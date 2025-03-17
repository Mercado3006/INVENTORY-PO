#include <iostream>
#include <string>
#include <cstdlib>
#include <ctime>
#include <iomanip>
#include <conio.h>
#include <limits>
#include <sstream>

using namespace std;

string intToString(int value) {
    stringstream ss;
    ss << value;
    return ss.str();
}

void clearScreen() { system("cls"); }
void waitForKey() {
    cout << "\nPress any key to continue...";
    cin.ignore(numeric_limits<streamsize>::max(), '\n');
    cin.get();
}

struct Component {
    string name;
    int quantity;
    string status;
};

class Unit {
public:
    Component components[7];
    int numComponents;

    Unit() {
        numComponents = 5;
        components[0] = {"Mouse", 1, randomStatus()};
        components[1] = {"Keyboard", 1, randomStatus()};
        components[2] = {"AVR", 1, randomStatus()};
        components[3] = {"HDMI", 1, randomStatus()};
        components[4] = {"System Unit", 1, randomStatus()};
    }

    string randomStatus() {
        return (rand() % 5 == 0) ? "Bad" : "Good";
    }

    void addComponent() {
        if (numComponents >= 7) {
            showMessage("Cannot add more components. Maximum limit reached.");
            return;
        }

        string name, status;
        int quantity;
        
        clearScreen();
        showHeader("Add New Component");
        
        cout << "Component name: ";
        cin >> name;
        
        cout << "Quantity: ";
        if (!(cin >> quantity) || quantity <= 0) {
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            showMessage("Invalid quantity value. Must be a positive number.");
            return;
        }
        
        cout << "Status (Good/Bad): ";
        cin >> status;
        
        if (status != "Good" && status != "Bad") {
            showMessage("Invalid status. Must be either 'Good' or 'Bad'.");
            return;
        }

        components[numComponents] = {name, quantity, status};
        numComponents++;

        showMessage("Component added successfully!");
    }

    void editComponent(int index) {
        if (index < 0 || index >= numComponents) {
            showMessage("Invalid component index.");
            return;
        }

        string name, status;
        int quantity;
        
        clearScreen();
        showHeader("Edit Component");
        
        cout << "Editing: " << components[index].name << endl;
        cout << "Current details:" << endl;
        cout << "  Name: " << components[index].name << endl;
        cout << "  Quantity: " << components[index].quantity << endl;
        cout << "  Status: " << components[index].status << endl << endl;

        cout << "New name (leave blank to keep current): ";
        cin.ignore();
        getline(cin, name);

        if (name.empty()) {
            name = components[index].name;
        }

        cout << "New quantity (0 to keep current): ";
        if (!(cin >> quantity)) {
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            showMessage("Invalid quantity value. Must be a number.");
            return;
        }
        
        if (quantity < 0) {
            showMessage("Quantity cannot be negative.");
            return;
        }
        
        if (quantity == 0) {
            quantity = components[index].quantity;
        }

        cout << "New status (Good/Bad, leave blank to keep current): ";
        cin >> status;

        if (status != "Good" && status != "Bad" && !status.empty()) {
            showMessage("Invalid status. Must be either 'Good' or 'Bad'.");
            return;
        }
        
        if (status.empty()) {
            status = components[index].status;
        }

        components[index] = {name, quantity, status};
        showMessage("Component updated successfully!");
    }

    void deleteComponent(int index) {
        if (index < 0 || index >= numComponents) {
            showMessage("Invalid component index.");
            return;
        }
        
        if (numComponents <= 1) {
            showMessage("Cannot delete the last component.");
            return;
        }

        clearScreen();
        showHeader("Delete Component");
        
        cout << "Are you sure you want to delete: " << components[index].name << "? (y/n): ";
        
        char confirm;
        cin >> confirm;
        
        if (confirm != 'y' && confirm != 'Y') {
            showMessage("Deletion cancelled.");
            return;
        }

        for (int i = index; i < numComponents - 1; ++i) {
            components[i] = components[i + 1];
        }

        numComponents--;
        showMessage("Component deleted successfully!");
    }

    void displayComponents(int unitNumber) {
        clearScreen();
        showHeader("UNIT " + intToString(unitNumber) + " - COMPUTER LAB 01");
        
        cout << "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^" << endl;
        cout << "¦ COMPONENT         ¦ QUANTITY   ¦ STATUS  ¦" << endl;
        cout << "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^" << endl;

        for (int i = 0; i < numComponents; ++i) {
            cout << "¦ " << left << setw(17) << components[i].name
                 << "¦ " << setw(10) << components[i].quantity
                 << "¦ " << setw(7) << components[i].status << "¦" << endl;
        }
        
        cout << "+******************************************+" << endl << endl;
    }

private:
    void showHeader(const string& title) {
        cout << "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^" << endl;
        cout << "¦ " << left << setw(41) << title << "¦" << endl;
        cout << "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^" << endl << endl;
    }
    
    void showMessage(const string& message) {
        clearScreen();
        cout << endl << endl;
        cout << "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^" << endl;
        cout << "¦ " << left << setw(41) << message << "¦" << endl;
        cout << "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^" << endl << endl;
        waitForKey();
    }
};

void showError(const string& message) {
    clearScreen();
    cout << endl << endl;
    cout << "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^" << endl;
    cout << "¦ ERROR: " << left << setw(34) << message << "¦" << endl;
    cout << "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^" << endl << endl;
    waitForKey();
}

void displayMainMenu() {
    clearScreen();
    cout << " _________________________________________________________________________   \n";
    cout << "^                                                                         ^ \n";
    cout << "| ^                           COMPUTER LABORATORY                       ^ | \n";
    cout << "^ |+-------------------------------------------------------------------+| ^ \n";
    cout << "| ^                                                                     ^ | \n";
    cout << "^ |      PC01     |     PC02     |    PC03    |    PC04    |     PC05   | ^ \n";
    cout << "| ^                                                                     ^ | \n";
    cout << "^ |+-------------------------------------------------------------------+| ^ \n";
    cout << "| ^                                                                     ^ | \n";
    cout << "^ |      PC06     |     PC07     |    PC08    |     PC09   |     PC10   | ^ \n";
    cout << "| ^                                                                     ^ | \n";
    cout << "^ |+--------------------------------------------------------------------| ^ \n";
    cout << "| ^                                                                     ^ | \n";
    cout << "^ |      PC11     |     PC12    |     PC13    |     PC14   |     PC15   | ^ \n";
    cout << "| ^                                                                     ^ | \n";
    cout << "^ |+--------------------------------------------------------------------| ^ \n";
    cout << "| ^                                                                     ^ | \n";
    cout << "^ |      PC16     |     PC17    |     PC18    |    PC19    |     PC20   | ^ \n";
    cout << "| ^                                                                     ^ | \n";
    cout << "^ |+-------------------------------------------------------------------+| ^ \n";
    cout << "| _______________________________________________________________________ | \n";
    cout << "                                                                            \n";
    
    cout << "***************************" << endl;
    cout << "& 1. Search Unit          &" << endl;
    cout << "& 2. Display All Units    &" << endl;
    cout << "& 3. Exit                 &" << endl;
    cout << "***************************" << endl << endl;
    
    cout << "Select an option: ";
}

void displayComponentMenu() {
    cout << endl;
    cout << "***************************" << endl;
    cout << "& 1. Add Component        &" << endl;
    cout << "& 2. Edit Component       &" << endl;
    cout << "& 3. Delete Component     &" << endl;
    cout << "& 4. Back to Main Menu    &" << endl;
    cout << "***************************" << endl << endl;
    
    cout << "Select an option: ";
}

int main() {
    srand(static_cast<unsigned>(time(0)));
    system("title Computer Lab Inventory System");
    
    int choice, unitChoice, componentChoice;
    Unit units[20];
    
    while (true) {
        displayMainMenu();
        
        choice = _getch() - '0';
        
        if (choice < 1 || choice > 3) {
            showError("Invalid option. Please select a valid option (1-3).");
            continue;
        }

        switch (choice) {
            case 1:
                clearScreen();
                cout << "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^" << endl;
                cout << "¦              SEARCH UNIT                    ¦" << endl;
                cout << "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^" << endl << endl;
                
                cout << "Enter unit number (1-20): ";
                
                if (!(cin >> unitChoice) || unitChoice < 1 || unitChoice > 20) {
                    cin.clear();
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');
                    showError("Invalid unit number. Must be between 1 and 20.");
                    continue;
                }
                
                units[unitChoice - 1].displayComponents(unitChoice);
                displayComponentMenu();
                
                componentChoice = _getch() - '0';
                
                if (componentChoice < 1 || componentChoice > 4) {
                    showError("Invalid option. Please select a valid option (1-4).");
                    continue;
                }
                
                switch (componentChoice) {
                    case 1:
                        units[unitChoice - 1].addComponent();
                        break;
                        
                    case 2:
                        clearScreen();
                        units[unitChoice - 1].displayComponents(unitChoice);
                        cout << "Enter component number to edit (1-" << units[unitChoice - 1].numComponents << "): ";
                        
                        int editIndex;
                        if (!(cin >> editIndex) || editIndex < 1 || editIndex > units[unitChoice - 1].numComponents) {
                            cin.clear();
                            cin.ignore(numeric_limits<streamsize>::max(), '\n');
                            showError("Invalid component number.");
                            continue;
                        }
                        
                        units[unitChoice - 1].editComponent(editIndex - 1);
                        break;
                        
                    case 3:
                        clearScreen();
                        units[unitChoice - 1].displayComponents(unitChoice);
                        cout << "Enter component number to delete (1-" << units[unitChoice - 1].numComponents << "): ";
                        
                        int deleteIndex;
                        if (!(cin >> deleteIndex) || deleteIndex < 1 || deleteIndex > units[unitChoice - 1].numComponents) {
                            cin.clear();
                            cin.ignore(numeric_limits<streamsize>::max(), '\n');
                            showError("Invalid component number.");
                            continue;
                        }
                        
                        units[unitChoice - 1].deleteComponent(deleteIndex - 1);
                        break;
                        
                    case 4:
                        break;
                }
                break;

            case 2:  // Display all units
                clearScreen();
                cout << "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^" << endl;
                cout << "¦             ALL UNITS REPORT                ¦" << endl;
                cout << "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^" << endl << endl;
                
                cout << "^^^^^^^^^^^^^^^^^^^^^^^^^" << endl;
                cout << "¦ UNIT NO.    ¦ STATUS  ¦" << endl;
                cout << "^^^^^^^^^^^^^^^^^^^^^^^^^" << endl;
                
                for (int i = 0; i < 20; ++i) {
                    cout << "¦ C" << setw(2) << setfill('0') << (i + 1) << "        ¦ " 
                         << setfill(' ') << setw(7) << units[i].components[0].status << "¦" << endl;
                }
                
                cout << "+-----------------------+" << endl;
                waitForKey();
                break;

            case 3:  // Exit
                clearScreen();
                cout << endl << endl;
                cout << "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^" << endl;
                cout << "¦    Thank you for using the Inventory System <3   ¦" << endl;
                cout << "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^" << endl;
                return 0;
        }
    }

    return 0; 
