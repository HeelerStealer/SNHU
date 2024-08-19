#include <iostream>
#include <fstream>
#include <map>
#include <string>

class ItemTracker {
private:
    std::map<std::string, int> itemFrequencyMap;

public:
    void loadItemsFromFile(const std::string& filename);
    int getItemFrequency(const std::string& itemName);
    void createBackupFile(const std::string& backupFilename);
    void displayAllItems();
    void displayHistogram();
};

void ItemTracker::loadItemsFromFile(const std::string& filename) {
    std::ifstream inputFile(filename);
    std::string item;

    if (inputFile.is_open()) {
        while (inputFile >> item) {
            itemFrequencyMap[item]++;
        }
        inputFile.close();
    }
    else {
        std::cerr << "Error: Unable to open file " << filename << std::endl;
    }
}

int ItemTracker::getItemFrequency(const std::string& itemName) {
    return itemFrequencyMap[itemName];
}

void ItemTracker::createBackupFile(const std::string& backupFilename) {
    std::ofstream backupFile(backupFilename);

    if (backupFile.is_open()) {
        for (const auto& item : itemFrequencyMap) {
            backupFile << item.first << " " << item.second << std::endl;
        }
        backupFile.close();
    }
    else {
        std::cerr << "Error: Unable to create backup file " << backupFilename << std::endl;
    }
}

void ItemTracker::displayAllItems() {
    for (const auto& item : itemFrequencyMap) {
        std::cout << item.first << " " << item.second << std::endl;
    }
}

void ItemTracker::displayHistogram() {
    for (const auto& item : itemFrequencyMap) {
        std::cout << item.first << " ";
        for (int i = 0; i < item.second; ++i) {
            std::cout << "*";
        }
        std::cout << std::endl;
    }
}

int main() {
    ItemTracker tracker;
    tracker.loadItemsFromFile("CS210_Project_Three_Input_File.txt");
    tracker.createBackupFile("frequency.dat");

    int choice = 0;
    while (choice != 4) {
        std::cout << "\nMenu:\n";
        std::cout << "1. Search for item frequency\n";
        std::cout << "2. Display all items and frequencies\n";
        std::cout << "3. Display histogram of items\n";
        std::cout << "4. Exit\n";
        std::cout << "Enter your choice: ";
        std::cin >> choice;

        switch (choice) {
        case 1: {
            std::string itemName;
            std::cout << "Enter item name: ";
            std::cin >> itemName;
            int frequency = tracker.getItemFrequency(itemName);
            std::cout << "Frequency of " << itemName << ": " << frequency << std::endl;
            break;
        }
        case 2:
            tracker.displayAllItems();
            break;
        case 3:
            tracker.displayHistogram();
            break;
        case 4:
            std::cout << "Exiting program.\n";
            break;
        default:
            std::cout << "Invalid choice. Please try again.\n";
        }
    }

    return 0;
}
