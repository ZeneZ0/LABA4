# LABA4
Абстрактный класс Sportsman:
import java.io.Serializable;

public abstract class Sportsman implements Serializable {
    protected String name;
    protected int age;
    protected String sportType;

    public Sportsman(String name, int age, String sportType) {
        this.name = name;
        this.age = age;
        this.sportType = sportType;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public String getSportType() {
        return sportType;
    }

    // Абстрактный метод, который будет по-разному реализован в наследниках
    public abstract void выполнить_упражнение();

    // Метод для отображения информации
    public abstract void displayInfo();
}
Класс Amateur:
public class Amateur extends Sportsman {
    private String уровень_подготовки;

    public Amateur(String name, int age, String sportType, String уровень_подготовки) {
        super(name, age, sportType);
        this.уровень_подготовки = уровень_подготовки;
    }

    @Override
    public void выполнить_упражнение() {
        System.out.println(name + " выполняет упражнение как любитель с уровнем " + уровень_подготовки);
    }

    @Override
    public void displayInfo() {
        System.out.println("Любитель:");
        System.out.println("Имя: " + name);
        System.out.println("Возраст: " + age);
        System.out.println("Вид спорта: " + sportType);
        System.out.println("Уровень подготовки: " + уровень_подготовки);
    }
}
Класс Professional:
public class Professional extends Sportsman {
    private String звание;
    private String разряд;

    public Professional(String name, int age, String sportType, String звание, String разряд) {
        super(name, age, sportType);
        this.звание = звание;
        this.разряд = разряд;
    }

    @Override
    public void выполнить_упражнение() {
        System.out.println(name + " выполняет профессиональное упражнение с разрядом " + разряд);
    }

    @Override
    public void displayInfo() {
        System.out.println("Профессионал:");
        System.out.println("Имя: " + name);
        System.out.println("Возраст: " + age);
        System.out.println("Вид спорта: " + sportType);
        System.out.println("Звание: " + звание);
        System.out.println("Разряд: " + разряд);
    }
}
Класс Trainer:
public class Trainer {
    private String name;
    private String специализация;
    private int стаж_работы;

    public Trainer(String name, String специализация, int стаж_работы) {
        this.name = name;
        this.специализация = специализация;
        this.стаж_работы = стаж_работы;
    }

    public void displayInfo() {
        System.out.println("Тренер:");
        System.out.println("Имя: " + name);
        System.out.println("Специализация: " + специализация);
        System.out.println("Стаж работы: " + стаж_работы + " лет");
    }

    public void подготовить_спортсмена(Sportsman sportsman) {
        System.out.println(name + " готовит спортсмена " + sportsman.getName());
    }
}
Класс SportSection:
import java.io.Serializable;
import java.util.ArrayList;
import java.util.List;

public class SportSection implements Serializable {
    private List<Sportsman> sportsmans = new ArrayList<>();
    private Trainer trainer;

    public void addSportsman(Sportsman sportsman) {
        sportsmans.add(sportsman);
    }

    public void removeSportsman(Sportsman sportsman) {
        sportsmans.remove(sportsman);
    }

    public Sportsman findSportsmanByName(String name) {
        for (Sportsman sportsman : sportsmans) {
            if (sportsman.getName().equalsIgnoreCase(name)) {
                return sportsman;
            }
        }
        return null;
    }

    public void setTrainer(Trainer trainer) {
        this.trainer = trainer;
    }

    public void displayAllSportsmans() {
        for (Sportsman sportsman : sportsmans) {
            sportsman.displayInfo();
            System.out.println("-------------------");
        }
    }
}
Файл SportsFileManager:
import java.io.*;

public class SportsFileManager {
    public static void saveSectionToFile(SportSection section, String fileName) {
        try (ObjectOutputStream outputStream = new ObjectOutputStream(new FileOutputStream(fileName))) {
            outputStream.writeObject(section);
            System.out.println("Секция спортсменов сохранена в файл: " + fileName);
        } catch (IOException e) {
            System.out.println("Ошибка при сохранении данных: " + e.getMessage());
        }
    }

    public static SportSection loadSectionFromFile(String fileName) {
        SportSection section = null;
        try (ObjectInputStream inputStream = new ObjectInputStream(new FileInputStream(fileName))) {
            section = (SportSection) inputStream.readObject();
            System.out.println("Секция спортсменов загружена из файла: " + fileName);
        } catch (IOException | ClassNotFoundException e) {
            System.out.println("Ошибка при загрузке данных: " + e.getMessage());
        }
        return section;
    }
}
Файл Menu:
import java.util.Scanner;
import java.util.InputMismatchException;

public class Menu {
    private SportSection sportSection;
    private Scanner scanner;

    public Menu(SportSection sportSection, Scanner scanner) {
        this.sportSection = sportSection;
        this.scanner = scanner;
    }

    public void displayMenu() {
        int choice = 0;
        do {
            System.out.println("\nМеню:");
            System.out.println("1. Добавить спортсмена");
            System.out.println("2. Показать всех спортсменов");
            System.out.println("3. Найти спортсмена");
            System.out.println("4. Удалить спортсмена");
            System.out.println("5. Добавить тренера");
            System.out.println("6. Сохранить в файл");
            System.out.println("7. Загрузить из файла");
            System.out.println("8. Выйти");

            try {
                System.out.print("Выберите действие: ");
                choice = scanner.nextInt();
                scanner.nextLine(); 

                switch (choice) {
                    case 1 -> addSportsman();
                    case 2 -> sportSection.displayAllSportsmans();
                    case 3 -> findSportsman();
                    case 4 -> removeSportsman();
                    case 5 -> addTrainer();
                    case 6 -> saveToFile();
                    case 7 -> loadFromFile();
                    case 8 -> System.out.println("Выход...");
                    default -> System.out.println("Некорректный выбор!");
                }
            } catch (InputMismatchException e) {
                System.out.println("Ошибка ввода. Пожалуйста, введите корректное число.");
                scanner.nextLine();
            }
        } while (choice != 8);
    }

    private void addSportsman() {
        try {
            System.out.println("Выберите тип спортсмена:");
            System.out.println("1. Любитель");
            System.out.println("2. Профессионал");

            int type = scanner.nextInt();
            scanner.nextLine(); 

            System.out.print("Введите имя: ");
            String name = scanner.nextLine();
            System.out.print("Введите возраст: ");
            int age = scanner.nextInt();
            scanner.nextLine(); 
            System.out.print("Введите вид спорта: ");
            String sportType = scanner.nextLine();

            Sportsman sportsman;
            switch (type) {
                case 1 -> {
                    System.out.print("Введите уровень подготовки: ");
                    String level = scanner.nextLine();
                    sportsman = new Amateur(name, age, sportType, level);
                }
                case 2 -> {
                    System.out.print("Введите звание: ");
                    String title = scanner.nextLine();
                    System.out.print("Введите разряд: ");
                    String category = scanner.nextLine();
                    sportsman = new Professional(name, age, sportType, title, category);
                }
                default -> {
                    System.out.println("Некорректный выбор.");
                    return;
                }
            }

            sportSection.addSportsman(sportsman);
            System.out.println("Спортсмен успешно добавлен.");
        } catch (InputMismatchException e) {
            System.out.println("Ошибка ввода. Пожалуйста, введите корректные данные.");
            scanner.nextLine();
        }
    }

    private void findSportsman() {
        System.out.print("Введите имя спортсмена для поиска: ");
        String name = scanner.nextLine();
        Sportsman sportsman = sportSection.findSportsmanByName(name);

        if (sportsman != null) {
            sportsman.displayInfo();
        } else {
            System.out.println("Спортсмен не найден.");
        }
    }

    private void removeSportsman() {
        System.out.print("Введите имя спортсмена для удаления: ");
        String name = scanner.nextLine();
        Sportsman sportsman = sportSection.findSportsmanByName(name);

        if (sportsman != null) {
            sportSection.removeSportsman(sportsman);
            System.out.println("Спортсмен успешно удалён.");
        } else {
            System.out.println("Спортсмен не найден.");
        }
    }

    private void addTrainer() {
        System.out.print("Введите имя тренера: ");
        String name = scanner.nextLine();
        System.out.print("Введите специализацию: ");
        String specialization = scanner.nextLine();
        System.out.print("Введите стаж работы: ");
        int experience = scanner.nextInt();
        scanner.nextLine();

        Trainer trainer = new Trainer(name, specialization, experience);
        sportSection.setTrainer(trainer);
        System.out.println("Тренер успешно добавлен.");
    }

    private void saveToFile() {
        System.out.print("Введите имя файла для сохранения: ");
        String fileName = scanner.nextLine();
        SportsFileManager.saveSectionToFile(sportSection, fileName);
    }

    private void loadFromFile() {
        System.out.print("Введите имя файла для загрузки: ");
        String fileName = scanner.nextLine();
        SportSection loadedSection = SportsFileManager.loadSectionFromFile(fileName);

        if (loadedSection != null) {
            this.sportSection = loadedSection;
            System.out.println("Спортивная секция успешно загружена.");
        }
    }
}
Файл Main:
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        SportSection section = new SportSection();
        Scanner scanner = new Scanner(System.in);
        Menu menu = new Menu(section, scanner);
        menu.displayMenu();
    }
}
