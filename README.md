#include <iostream>
#include <vector>
#include <iomanip>
#include <fstream>
using namespace std;

// Item structure
struct Item {
    string name;
    int quantity;
    double price;
};

// Bill Management Class
class BillManagement {
private:
    vector<Item> items;
    double totalAmount;

public:
    BillManagement() : totalAmount(0.0) {}

    // Add an item to the bill
    void addItem(const string& name, int quantity, double price) {
        items.push_back({name, quantity, price});
        totalAmount += quantity * price;
    }

    // Display the bill
    void displayBill() {
        cout << "\n---------- Bill Details ----------\n";
        cout << left << setw(20) << "Item Name"
             << setw(10) << "Quantity"
             << setw(10) << "Price"
             << setw(10) << "Total" << endl;
        cout << "-----------------------------------\n";

        for (const auto& item : items) {
            cout << left << setw(20) << item.name
                 << setw(10) << item.quantity
                 << setw(10) << fixed << setprecision(2) << item.price
                 << setw(10) << item.quantity * item.price << endl;
        }

        cout << "-----------------------------------\n";
        cout << "Total Amount: " << fixed << setprecision(2) << totalAmount << endl;
    }

    // Save bill to file
    void saveBillToFile(const string& filename) {
        ofstream file(filename);
        if (file.is_open()) {
            file << "---------- Bill Details ----------\n";
            file << left << setw(20) << "Item Name"
                 << setw(10) << "Quantity"
                 << setw(10) << "Price"
                 << setw(10) << "Total" << endl;
            file << "-----------------------------------\n";

            for (const auto& item : items) {
                file << left << setw(20) << item.name
                     << setw(10) << item.quantity
                     << setw(10) << fixed << setprecision(2) << item.price
                     << setw(10) << item.quantity * item.price << endl;
            }

            file << "-----------------------------------\n";
            file << "Total Amount: " << fixed << setprecision(2) << totalAmount << endl;
            file.close();
            cout << "Bill saved to " << filename << " successfully.\n";
        } else {
            cout << "Error: Unable to open file.\n";
        }
    }
};

int main() {
    BillManagement billManager;
    int choice;

    do {
        cout << "\n---------- Bill Management ----------\n";
        cout << "1. Add Item\n";
        cout << "2. Display Bill\n";
        cout << "3. Save Bill to File\n";
        cout << "4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1: {
                string name;
                int quantity;
                double price;
                cout << "Enter item name: ";
                cin >> ws; // To ignore leading whitespace
                getline(cin, name);
                cout << "Enter quantity: ";
                cin >> quantity;
                cout << "Enter price: ";
                cin >> price;
                billManager.addItem(name, quantity, price);
                break;
            }
            case 2:
                billManager.displayBill();
                break;
            case 3: {
                string filename;
                cout << "Enter filename to save the bill: ";
                cin >> filename;
                billManager.saveBillToFile(filename);
                break;
            }
            case 4:
                cout << "Exiting the program. Thank you!\n";
                break;
            default:
                cout << "Invalid choice. Please try again.\n";
        }
    } while (choice != 4);

    return 0;
}
