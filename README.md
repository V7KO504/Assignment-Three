#include <iostream>
#include <fstream>
#include <stdexcept>

using namespace std;

class Student {
private:
    string name;
    int age;
    float grade;

public:
    Student(string n = "", int a = 0, float g = 0.0f) : name(n), age(a), grade(g) {}

    void display() const {
        cout << "Name: " << name << ", Age: " << age << ", Grade: " << grade << endl;
    }

    void writeToFile(const string& filename) {
        ofstream outFile(filename);
        if (!outFile) throw runtime_error("Error: Unable to open file for writing.");
        outFile << name << " " << age << " " << grade;
        outFile.close();
    }

    void readFromFile(const string& filename) {
        ifstream inFile(filename);
        if (!inFile) throw runtime_error("Error: Unable to open file for reading.");
        if (inFile.peek() == ifstream::traits_type::eof()) throw logic_error("Error: File is empty.");
        inFile >> name >> age >> grade;
        if (inFile.fail()) throw invalid_argument("Error: File data is not in the expected format.");
        if (name.empty() || age <= 0 || grade < 0.0f) throw length_error("Error: File is corrupted or missing fields.");
        inFile.close();
    }
};

int main() {
    try {
        Student student("Frank", 20, 85.5f);
        student.writeToFile("student_data.txt");
        cout << "Student data written to file successfully." << endl;

        Student student2;
        student2.readFromFile("student_data.txt");
        cout << "Student data read from file successfully." << endl;

        student2.display();
    } catch (const exception& e) {
        cerr << e.what() << endl;
    }

    return 0;
}
