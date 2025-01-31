# Diary-Simple-ConsoleBase-Management
Sure! Here's a revised version of your Diary program with a cleaner and more attractive console output. I’ve added some formatting, colors, and clear separation of options to make the user interface more visually appealing.

import java.io.*;
import java.util.*;
import java.util.concurrent.TimeUnit;

public class Diary {
    private static final String FILE_NAME = "diary.txt";
    private static final Scanner scanner = new Scanner(System.in);
    private static final List<String> notes = new ArrayList<>();

    public static void main(String[] args) {
        loadNotes();

        while (true) {
            showMenu();
            String choice = scanner.nextLine();

            switch (choice) {
                case "1":
                    addNote();
                    break;
                case "2":
                    editNote();
                    break;
                case "3":
                    deleteNote();
                    break;
                case "4":
                    viewNotes();
                    break;
                case "5":
                    saveNotes();
                    System.out.println("\nGoodbye! Your notes are saved.\n");
                    delay(1);  // Delay to make the exit message clearer
                    return;
                default:
                    System.out.println("\nInvalid option. Please choose again.\n");
                    delay(1);
            }
        }
    }

    private static void showMenu() {
        System.out.println("\n" + "=".repeat(30));
        System.out.println("\033[1;34m*** Welcome to Your Simple Diary ***\033[0m");
        System.out.println("=".repeat(30));
        System.out.println("\n1. \033[1;32mAdd Note\033[0m");
        System.out.println("2. \033[1;33mEdit Note\033[0m");
        System.out.println("3. \033[1;31mDelete Note\033[0m");
        System.out.println("4. \033[1;36mView Notes\033[0m");
        System.out.println("5. \033[1;35mExit\033[0m");
        System.out.print("\nEnter your choice: ");
    }

    private static void addNote() {
        System.out.print("\nEnter your note: ");
        String note = scanner.nextLine();
        notes.add(note);
        System.out.println("\n\033[1;32mNote added successfully!\033[0m\n");
        delay(1);
    }

    private static void editNote() {
        if (notes.isEmpty()) {
            System.out.println("\nNo notes available to edit.\n");
            return;
        }

        viewNotes();
        System.out.print("\nEnter the number of the note to edit: ");
        int noteIndex = Integer.parseInt(scanner.nextLine()) - 1;
        if (noteIndex >= 0 && noteIndex < notes.size()) {
            System.out.print("\nEnter the new content for the note: ");
            String newNote = scanner.nextLine();
            notes.set(noteIndex, newNote);
            System.out.println("\n\033[1;33mNote updated successfully!\033[0m\n");
            delay(1);
        } else {
            System.out.println("\n\033[1;31mInvalid note number.\033[0m\n");
        }
    }

    private static void deleteNote() {
        if (notes.isEmpty()) {
            System.out.println("\nNo notes available to delete.\n");
            return;
        }

        viewNotes();
        System.out.print("\nEnter the number of the note to delete: ");
        int noteIndex = Integer.parseInt(scanner.nextLine()) - 1;
        if (noteIndex >= 0 && noteIndex < notes.size()) {
            notes.remove(noteIndex);
            System.out.println("\n\033[1;31mNote deleted successfully!\033[0m\n");
            delay(1);
        } else {
            System.out.println("\n\033[1;31mInvalid note number.\033[0m\n");
        }
    }

    private static void viewNotes() {
        if (notes.isEmpty()) {
            System.out.println("\n\033[1;33mNo notes available.\033[0m\n");
        } else {
            System.out.println("\n\033[1;36m--- Your Notes ---\033[0m");
            for (int i = 0; i < notes.size(); i++) {
                System.out.println("\033[1;37m" + (i + 1) + ". " + notes.get(i) + "\033[0m");
            }
            System.out.println();
        }
    }

    private static void loadNotes() {
        try {
            File file = new File(FILE_NAME);
            if (file.exists()) {
                BufferedReader reader = new BufferedReader(new FileReader(file));
                String line;
                while ((line = reader.readLine()) != null) {
                    notes.add(line);
                }
                reader.close();
            }
        } catch (IOException e) {
            System.out.println("\n\033[1;31mError loading notes.\033[0m");
        }
    }

    private static void saveNotes() {
        try {
            BufferedWriter writer = new BufferedWriter(new FileWriter(FILE_NAME));
            for (String note : notes) {
                writer.write(note);
                writer.newLine();
            }
            writer.close();
        } catch (IOException e) {
            System.out.println("\n\033[1;31mError saving notes.\033[0m");
        }
    }

    private static void delay(int seconds) {
        try {
            TimeUnit.SECONDS.sleep(seconds);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}

