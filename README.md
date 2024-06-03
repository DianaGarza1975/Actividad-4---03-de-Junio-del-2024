# Actividad-4---03-de-Junio-del-2024
Demostrar los conocimientos sobre el manejo de archivos/Agenda telefónica
import java.io.*;
import java.util.*;

public class AddressBook {
    private Map<String, String> contacts;
    private String filename;

    public AddressBook(String filename) {
        this.filename = filename;
        this.contacts = new HashMap<>();
    }

    public void load() {
        try (BufferedReader br = new BufferedReader(new FileReader(filename))) {
            String line;
            while ((line = br.readLine()) != null) {
                String[] parts = line.split(",");
                contacts.put(parts[0], parts[1]);
            }
        } catch (FileNotFoundException e) {
            System.err.println("El archivo " + filename + " no fue encontrado.");
            createEmptyFile();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public void save() {
        try (BufferedWriter bw = new BufferedWriter(new FileWriter(filename))) {
            for (Map.Entry<String, String> entry : contacts.entrySet()) {
                bw.write(entry.getKey() + "," + entry.getValue() + "\n");
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public void list() {
        System.out.println("Contactos:");
        for (Map.Entry<String, String> entry : contacts.entrySet()) {
            System.out.println(entry.getKey() + " : " + entry.getValue());
        }
    }

    public void create(String number, String name) {
        contacts.put(number, name);
    }

    public void delete(String number) {
        contacts.remove(number);
    }

    private void createEmptyFile() {
        try {
            File file = new File(filename);
            if (file.createNewFile()) {
                System.out.println("Archivo " + filename + " creado exitosamente.");
            } else {
                System.out.println("El archivo " + filename + " ya existe.");
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        AddressBook addressBook = new AddressBook("contacts.txt");
        addressBook.load();

        Scanner scanner = new Scanner(System.in);
        int choice = 0;

        while (choice != 4) {
            System.out.println("Menú:");
            System.out.println("1. Mostrar contactos");
            System.out.println("2. Agregar contacto");
            System.out.println("3. Eliminar contacto");
            System.out.println("4. Salir");
            System.out.print("Seleccione una opción: ");
            choice = scanner.nextInt();
            scanner.nextLine(); // consume newline character

            switch (choice) {
                case 1:
                    addressBook.list();
                    break;
                case 2:
                    System.out.print("Ingrese el número de teléfono: ");
                    String number = scanner.nextLine();
                    System.out.print("Ingrese el nombre: ");
                    String name = scanner.nextLine();
                    addressBook.create(number, name);
                    break;
                case 3:
                    System.out.print("Ingrese el número de teléfono a eliminar: ");
                    String numberToDelete = scanner.nextLine();
                    addressBook.delete(numberToDelete);
                    break;
                case 4:
                    addressBook.save();
                    System.out.println("Saliendo...");
                    break;
                default:
                    System.out.println("Opción no válida, intente de nuevo.");
                    break;
            }
        }
        scanner.close();
    }
}
