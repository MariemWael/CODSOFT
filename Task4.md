#include <iostream>
#include <vector>
#include <string>

class Task {
public:
    std::string description;
    bool completed;

    Task(const std::string& desc) : description(desc), completed(false) {}
};

class ToDoList {
private:
    std::vector<Task> tasks;

public:
    void addTask(const std::string& description) {
        tasks.emplace_back(description);
        std::cout << "Task added successfully." << std::endl;
    }

    void viewTasks() const {
        if (tasks.empty()) {
            std::cout << "No tasks in the list." << std::endl;
        } else {
            for (size_t i = 0; i < tasks.size(); ++i) {
                std::cout << i + 1 << ". [" << (tasks[i].completed ? "X" : " ") << "] " << tasks[i].description << std::endl;
            }
        }
    }

    void markTaskCompleted(size_t index) {
        if (index < 1 || index > tasks.size()) {
            std::cout << "Invalid task number." << std::endl;
        } else {
            tasks[index - 1].completed = true;
            std::cout << "Task marked as completed." << std::endl;
        }
    }

    void removeTask(size_t index) {
        if (index < 1 || index > tasks.size()) {
            std::cout << "Invalid task number." << std::endl;
        } else {
            tasks.erase(tasks.begin() + index - 1);
            std::cout << "Task removed successfully." << std::endl;
        }
    }
};

void showMenu() {
    std::cout << "\nTo-Do List Manager\n";
    std::cout << "1. Add Task\n";
    std::cout << "2. View Tasks\n";
    std::cout << "3. Mark Task as Completed\n";
    std::cout << "4. Remove Task\n";
    std::cout << "5. Exit\n";
    std::cout << "Enter your choice: ";
}

int main() {
    ToDoList toDoList;
    int choice;
    std::string description;
    size_t taskNumber;

    while (true) {
        showMenu();
        std::cin >> choice;

        switch (choice) {
            case 1:
                std::cin.ignore(); 
                std::cout << "Enter task description: ";
                std::getline(std::cin, description);
                toDoList.addTask(description);
                break;

            case 2:
                toDoList.viewTasks();
                break;

            case 3:
                std::cout << "Enter task number to mark as completed: ";
                std::cin >> taskNumber;
                toDoList.markTaskCompleted(taskNumber);
                break;

            case 4:
                std::cout << "Enter task number to remove: ";
                std::cin >> taskNumber;
                toDoList.removeTask(taskNumber);
                break;

            case 5:
                std::cout << "Exiting the program." << std::endl;
                return 0;

            default:
                std::cout << "Invalid choice. Please try again." << std::endl;
        }
    }

    return 0;
}

https://www.linkedin.com/posts/mariem-wael-308883266_codsoft-activity-7206588558302691330-U2q8?utm_source=share&utm_medium=member_android
