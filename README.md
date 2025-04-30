# Bibliotecapoo

Cortes Santos Daniel Enrique
Moran Hernández Angel Fernando
Vázquez Villagrán Jorge Eduardo

Link Github:
https://github.com/AngelF172004/Bibliotecapoo

Link Planner:
A

Capturas del Planner:
A

Archivo .jar:
A

Código fuente:
import java.util.Scanner;
import java.util.List;
import java.util.ArrayList;

abstract class RecursoBibliografico {
    protected String titulo;
    protected String codigo;

    public RecursoBibliografico(String titulo, String codigo) {
        this.titulo = titulo;
        this.codigo = codigo;
    }

    public String getTitulo() {
        return titulo;
    }

    public String getCodigo() {
        return codigo;
    }

    public abstract void mostrarDetalle();
}

class Libro extends RecursoBibliografico {
    private String autor;
    private boolean prestado;

    public Libro(String titulo, String autor, String isbn) {
        super(titulo, isbn);
        this.autor = autor;
        this.prestado = false;
    }

    public boolean isPrestado() {
        return prestado;
    }

    public void setPrestado(boolean prestado) {
        this.prestado = prestado;
    }

    @Override
    public void mostrarDetalle() {
        System.out.println("Libro: " + titulo + " | Autor: " + autor + " | ISBN: " + codigo + " | Prestado: " + prestado);
    }
}

class Revista extends RecursoBibliografico {
    private int anioPublicacion;
    private int ejemplares;

    public Revista(String titulo, String issn, int anioPublicacion, int ejemplares) {
        super(titulo, issn);
        this.anioPublicacion = anioPublicacion;
        this.ejemplares = ejemplares;
    }

    public int getEjemplares() {
        return ejemplares;
    }

    public void setEjemplares(int ejemplares) {
        this.ejemplares = ejemplares;
    }

    @Override
    public void mostrarDetalle() {
        System.out.println("Revista: " + titulo + " | ISSN: " + codigo + " | Año: " + anioPublicacion + " | Ejemplares: " + ejemplares);
    }
}

class Usuario {
    private String nombre;
    private List<RecursoBibliografico> recursosPrestados;

    public Usuario(String nombre) {
        this.nombre = nombre;
        this.recursosPrestados = new ArrayList<>();
    }

    public void PrestarRecurso(RecursoBibliografico recurso) {
        if (recurso instanceof Libro) {
            Libro libro = (Libro) recurso;
            if (!libro.isPrestado()) {
                libro.setPrestado(true);
                recursosPrestados.add(recurso);
                System.out.println("Préstamo exitoso de libro: " + libro.getTitulo());
            } else {
                System.out.println("El libro no está disponible.");
            }
        } else if (recurso instanceof Revista) {
            Revista revista = (Revista) recurso;
            if (revista.getEjemplares() > 0) {
                revista.setEjemplares(revista.getEjemplares() - 1);
                recursosPrestados.add(recurso);
                System.out.println("Préstamo exitoso de revista: " + revista.getTitulo());
            } else {
                System.out.println("No hay ejemplares disponibles.");
            }
        }
    }

    public void DevolverRecurso(RecursoBibliografico recurso) {
        if (recursosPrestados.remove(recurso)) {
            if (recurso instanceof Libro) {
                ((Libro) recurso).setPrestado(false);
                System.out.println("Devolución exitosa de libro: " + recurso.getTitulo());
            } else if (recurso instanceof Revista) {
                Revista revista = (Revista) recurso;
                revista.setEjemplares(revista.getEjemplares() + 1);
                System.out.println("Devolución exitosa de revista: " + recurso.getTitulo());
            }
        } else {
            System.out.println("Este recurso no está prestado por el usuario.");
        }
    }

    public void MostrarPrestados() {
        if (recursosPrestados.isEmpty()) {
            System.out.println("\n\nNo tienes préstamos");
        }
        else{
            System.out.println("\n\nRecursos prestados a " + nombre + ":");
            for (RecursoBibliografico r : recursosPrestados) {
                r.mostrarDetalle();
            }
        }
    }
}

public class BibliotecaApp {
    public static void main(String[] args) {
        Scanner entrada = new Scanner(System.in);
        List<RecursoBibliografico> recursos = new ArrayList<>();
        recursos.add(new Libro("El Quijote", "Miguel de Cervantes", "978-8491050275"));
        recursos.add(new Libro("Cien Años de Soledad", "Gabriel García Márquez", "978-0307474728"));
        recursos.add(new Libro("Don Juan Tenorio", "José Zorrilla", "978-8497408579"));
        recursos.add(new Libro("La Odisea", "Homero", "978-0140268867"));
        recursos.add(new Libro("1984", "George Orwell", "978-0451524935"));
        recursos.add(new Revista("National Geographic", "0027-9358", 2024, 3));
        recursos.add(new Revista("Time", "0040-781X", 2023, 2));
        recursos.add(new Revista("Scientific American", "0036-8733", 2022, 1));

        Usuario usuario = new Usuario("Usuario1");
        int opcion;
        do {
            System.out.println("\n--- Biblioteca ---");
            System.out.println("1. Listar recursos");
            System.out.println("2. Prestar recurso");
            System.out.println("3. Devolver recurso");
            System.out.println("4. Mostrar prestados");
            System.out.println("5. Salir");
            System.out.print("Seleccione una opción: ");
            opcion = entrada.nextInt();
            switch (opcion) {
                case 1:
                    System.out.println("\n");
                    for (int i = 0; i < recursos.size(); i++) {
                        System.out.print((i + 1) + ". ");
                        recursos.get(i).mostrarDetalle();
                    }
                    break;
                case 2:
                    System.out.print("\nNúmero de recurso a prestar: ");
                    int numPrestamo = entrada.nextInt() - 1;
                    if (numPrestamo >= 0 && numPrestamo < recursos.size()) {
                        usuario.PrestarRecurso(recursos.get(numPrestamo));
                    } else {
                        System.out.println("Opción no válida.");
                    }
                    break;
                case 3:
                    System.out.print("\n\nNúmero de recurso a devolver: ");
                    int numDevolucion = entrada.nextInt() - 1;
                    if (numDevolucion >= 0 && numDevolucion < recursos.size()) {
                        usuario.DevolverRecurso(recursos.get(numDevolucion));
                    } else {
                        System.out.println("Opción no válida.");
                    }
                    break;
                case 4:
                    usuario.MostrarPrestados();
                    break;
                case 5:
                    System.out.println("Saliendo...");
                    break;
                default:
                    System.out.println("Opción no válida.");
            }
        } while (opcion != 5);
        entrada.close();
    }
}
