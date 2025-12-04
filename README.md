using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ExamenFinal2._0
{
    internal class Program
    {
        // MATRIZ DE RESERVAS: 2 filas (0=mañana, 1=tarde) × 20 columnas
        static string[,] nombres = new string[2, 20];
        static string[,] combosReservados = new string[2, 20];

        // MENÚ DE COMBOS
        static string[] combos =
        {
            "COMBO 1: CAFÉ + PAN",
            "COMBO 2: JUGO + SÁNDWICH",
            "COMBO 3: TÉ + GALLETAS"
        };

        static double[] precios = { 4.50, 6.00, 3.50 };

        static void Main(string[] args)
        {
            int opcion;

            do
            {
                Console.Clear();
                Console.WriteLine("=== SISTEMA DE RESERVAS - CAFETERÍA UNIVERSITARIA ===");
                Console.WriteLine("1. Mostrar menú de combos");
                Console.WriteLine("2. Registrar reserva");
                Console.WriteLine("3. Cancelar reserva");
                Console.WriteLine("4. Listar reservas por turno");
                Console.WriteLine("5. Reporte de ingresos");
                Console.WriteLine("6. Buscar reserva por nombre");
                Console.WriteLine("7. Salir");
                Console.Write("Seleccione opción: ");

                int.TryParse(Console.ReadLine(), out opcion);

                Console.Clear();

                switch (opcion)
                {
                    case 1: MostrarMenuCombos(); break;
                    case 2: RegistrarReserva(); break;
                    case 3: CancelarReserva(); break;
                    case 4: ListarReservas(); break;
                    case 5: ReporteIngresos(); break;
                    case 6: BuscarReserva(); break;
                }

                if (opcion != 7)
                {
                    Console.WriteLine("\nPresione una tecla para continuar...");
                    Console.ReadKey();
                }

            } while (opcion != 7);
        }

        // --------------------------------------------------
        // FUNCIÓN 1: MOSTRAR MENU DE COMBOS
        // --------------------------------------------------
        static void MostrarMenuCombos()
        {
            Console.WriteLine("--- MENÚ DE COMBOS ---\n");
            for (int i = 0; i < combos.Length; i++)
            {
                Console.WriteLine($"{i + 1}. {combos[i]} - S/. {precios[i]}");
            }
        }

        // --------------------------------------------------
        // FUNCIÓN 2: REGISTRAR RESERVA
        // --------------------------------------------------
        static void RegistrarReserva()
        {
            MostrarMenuCombos();

            Console.Write("\nNOMBRE DEL ESTUDIANTE: ");
            string nombre = Console.ReadLine().ToUpper(); // ← CONVERTIDO A MAYÚSCULAS

            Console.Write("Seleccione turno (0=mañana, 1=tarde): ");
            int turno = int.Parse(Console.ReadLine());

            if (turno < 0 || turno > 1)
            {
                Console.WriteLine("Turno inválido.");
                return;
            }

            Console.Write("Seleccione combo (1-3): ");
            int combo = int.Parse(Console.ReadLine()) - 1;

            if (combo < 0 || combo >= combos.Length)
            {
                Console.WriteLine("Combo inválido.");
                return;
            }

            // Buscar espacio disponible
            for (int i = 0; i < 20; i++)
            {
                if (string.IsNullOrEmpty(nombres[turno, i]))
                {
                    nombres[turno, i] = nombre;
                    combosReservados[turno, i] = combos[combo];

                    Console.WriteLine("\nReserva registrada con éxito.");
                    return;
                }
            }

            Console.WriteLine("\nNo hay cupos disponibles para este turno.");
        }

        // --------------------------------------------------
        // FUNCIÓN 3: CANCELAR RESERVA
        // --------------------------------------------------
        static void CancelarReserva()
        {
            Console.Write("NOMBRE DEL ESTUDIANTE A CANCELAR: ");
            string nombre = Console.ReadLine().ToUpper();

            for (int t = 0; t < 2; t++)
            {
                for (int i = 0; i < 20; i++)
                {
                    if (nombres[t, i] == nombre)
                    {
                        nombres[t, i] = null;
                        combosReservados[t, i] = null;

                        Console.WriteLine("Reserva cancelada.");
                        return;
                    }
                }
            }

            Console.WriteLine("No se encontró la reserva.");
        }

        // --------------------------------------------------
        // FUNCIÓN 4: LISTAR RESERVAS POR TURNO
        // --------------------------------------------------
        static void ListarReservas()
        {
            Console.Write("Ingrese turno a listar (0=mañana, 1=tarde): ");
            int turno = int.Parse(Console.ReadLine());

            Console.WriteLine(turno == 0 ? "\n--- RESERVAS MAÑANA ---" : "\n--- RESERVAS TARDE ---");

            bool hay = false;

            for (int i = 0; i < 20; i++)
            {
                if (!string.IsNullOrEmpty(nombres[turno, i]))
                {
                    Console.WriteLine($"{i + 1}. {nombres[turno, i]} - {combosReservados[turno, i]}");
                    hay = true;
                }
            }

            if (!hay)
                Console.WriteLine("No hay reservas en este turno.");
        }

        // --------------------------------------------------
        // FUNCIÓN 5: REPORTE DE INGRESOS
        // --------------------------------------------------
        static void ReporteIngresos()
        {
            double totalM = 0, totalT = 0;

            for (int t = 0; t < 2; t++)
            {
                for (int i = 0; i < 20; i++)
                {
                    if (!string.IsNullOrEmpty(combosReservados[t, i]))
                    {
                        for (int c = 0; c < combos.Length; c++)
                        {
                            if (combosReservados[t, i] == combos[c])
                            {
                                if (t == 0) totalM += precios[c];
                                else totalT += precios[c];
                            }
                        }
                    }
                }
            }

            Console.WriteLine("\n--- REPORTES ---");
            Console.WriteLine($"Ingresos turno mañana: S/. {totalM}");
            Console.WriteLine($"Ingresos turno tarde:   S/. {totalT}");
            Console.WriteLine($"TOTAL:                  S/. {totalM + totalT}");
        }

        // --------------------------------------------------
        // FUNCIÓN 6: BUSCAR RESERVA POR NOMBRE
        // --------------------------------------------------
        static void BuscarReserva()
        {
            Console.Write("Nombre a buscar: ");
            string nombre = Console.ReadLine().ToUpper();

            for (int t = 0; t < 2; t++)
            {
                for (int i = 0; i < 20; i++)
                {
                    if (nombres[t, i] == nombre)
                    {
                        Console.WriteLine(
                            $"Encontrado en turno {(t == 0 ? "MAÑANA" : "TARDE")} - Combo: {combosReservados[t, i]}"
                        );
                        return;
                    }
                }
            }

            Console.WriteLine("Reserva no encontrada.");
        }
    }
}
