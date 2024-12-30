
#include <iostream>
#include <fstream>
#include <vector>
#include <cstring>
using namespace std;

class CommunityMember {
protected:
 int id;
 string fName;
 string lName;
 string address;
 string cellPhone;
public:
 CommunityMember(int memberId = 0, string firstName = "",
string lastName = "", string memberAddress = "", string
memberCellPhone = "")
 : id(memberId), fName(firstName), lName(lastName),
address(memberAddress), cellPhone(memberCellPhone) {}
 void setId(int memberId) {
 id = memberId;
 }
 int getId() const {
 return id;
 }
 void setFirstName(string firstName) {
 fName = firstName;
 }
 std::string getFirstName() const {
 return fName;
 }
 void setLastName(string lastName) {
 lName = lastName;
 }
 string getLastName() const {
 return lName;
 }
 void setAddress(string memberAddress) {
 address = memberAddress;
 }
 string getAddress() const {
 return address;
 }
 void setCellPhone(string memberCellPhone) {
 cellPhone = memberCellPhone;
 }
 string getCellPhone() const {
 return cellPhone;
 }
 virtual void readData() {
 cout << "Enter ID: ";
 cin >> id;
 cout << "Enter first name: ";
 cin >> fName;
 cout << "Enter last name: ";
 cin >> lName;
 cout << "Enter address: ";
 cin.ignore();
 getline(cin, address);
 cout << "Enter cell phone: ";
 cin >> cellPhone;
 }
 virtual void print() const {
 cout << "ID: " << id << endl;
 cout << "First Name: " << fName << endl;
 cout << "Last Name: " << lName << endl;
 cout << "Address: " << address << endl;
 cout << "Cell Phone: " << cellPhone << endl;
 }
 virtual void printToFile(ofstream& outFile) const {
 outFile.write(reinterpret_cast<const char*>(&id),
sizeof(id));
 outFile.write(fName.c_str(), fName.size() + 1);
 outFile.write(lName.c_str(), lName.size() + 1);
 outFile.write(address.c_str(), address.size() + 1);
 outFile.write(cellPhone.c_str(), cellPhone.size() + 1);
 }
 virtual void readFromFile(ifstream& inFile) {
 inFile.read(reinterpret_cast<char*>(&id), sizeof(id));
 char buffer[256];
 inFile.getline(buffer, sizeof(buffer));
 fName = buffer;
 inFile.getline(buffer, sizeof(buffer));
 lName = buffer;
 inFile.getline(buffer, sizeof(buffer));
 address = buffer;
 inFile.getline(buffer, sizeof(buffer));
 cellPhone = buffer;
 }
 virtual ~CommunityMember() {}
 bool operator==(const CommunityMember& other) const {
 return this->getId() == other.getId();
 }
};


class Alumnus : public CommunityMember {
private:
 unsigned int yearOfGraduation;
 char currentJob[11];
public:
 Alumnus(int memberId = 0, string firstName = "", string
lastName = "", string memberAddress = "", string memberCellPhone
= "", unsigned int yearOfGraduation = 0, const char* currentJob
= "")
 : CommunityMember(memberId, firstName, lastName,
memberAddress, memberCellPhone),
yearOfGraduation(yearOfGraduation) {
 strncpy(this->currentJob, currentJob, 10);
 this->currentJob[10] = '\0'; // Ensure null termination
 }
 unsigned int getYearOfGraduation() const {
 return yearOfGraduation;
 }
 void setYearOfGraduation(unsigned int yearOfGraduation) {
 this->yearOfGraduation = yearOfGraduation;
 }
 const char* getCurrentJob() const {
 return currentJob;
 }
 void setCurrentJob(const char* currentJob) {
 strncpy(this->currentJob, currentJob, 10);
 this->currentJob[10] = '\0';
 }
 bool operator==(const Alumnus& other) const {
 return CommunityMember::operator==(other) &&
yearOfGraduation == other.yearOfGraduation && strcmp(currentJob,
other.currentJob) == 0;
 }
 void readData() override {
 CommunityMember::readData();
 cout << "Enter year of graduation: ";
 cin >> yearOfGraduation;
 cout << "Enter current job: ";
 cin.ignore();
 cin.getline(currentJob, 11);
 }
 void print() const override {
 CommunityMember::print();
 cout << "Year of Graduation: " << yearOfGraduation <<
endl;
 cout << "Current Job: " << currentJob << endl;
 }
 void printToFile(ofstream& outFile) const override {
 CommunityMember::printToFile(outFile);
 outFile.write(reinterpret_cast<const
char*>(&yearOfGraduation), sizeof(yearOfGraduation));
 outFile.write(reinterpret_cast<const char*>(currentJob),
sizeof(currentJob));
 }
 void readFromFile(ifstream& inFile) override {
 CommunityMember::readFromFile(inFile);
 inFile.read(reinterpret_cast<char*>(&yearOfGraduation),
sizeof(yearOfGraduation));
 inFile.read(reinterpret_cast<char*>(currentJob),
sizeof(currentJob));
 }
};


class Student : public CommunityMember { 
    
protected:
    double gpa;
    unsigned int courseLoadHours;

public:
   Student(unsigned long long id = 0, const string& fname = "", const string& lname = "", const string& address = "",
        const string& cellPhone = "", double gpa = 0.0, unsigned int courseLoadHours = 0)
    : CommunityMember(id, fname, lname, address, cellPhone), gpa(gpa), courseLoadHours(courseLoadHours) {}


double getgpa() const { 
        return gpa; 
    } 
void setgpa(double gpa) { 
        this->gpa = gpa; 
    }
    
unsigned int getcourseLoadHours() const { 
        return courseLoadHours; 
    } 
void setcourseLoadHours(unsigned int courseLoadHours) { 
        this->courseLoadHours = courseLoadHours; 
    } 

bool operator==(const Student& other) const {
    return this->id == other.id; 
}

void readData() override {
  CommunityMember::readData();
cout << "Enter gpa: ";
cin>>gpa;
cout << "Enter course Load Hours: ";
cin>>courseLoadHours;
}

void print() const override {
  CommunityMember::print();
cout << "gpa : " << gpa;
cout << "course Load Hours: " << courseLoadHours ;
}


    void printToFile(ofstream& outFile) const override { 
        CommunityMember::printToFile(outFile); 
        outFile.write(reinterpret_cast<const char*>(&gpa), sizeof(gpa));  
        outFile.write(reinterpret_cast<const char*>(&courseLoadHours), sizeof(courseLoadHours));  
    }
    
    
   
    void readFromFile(ifstream& inFile) override { 
        CommunityMember::readFromFile(inFile);
        inFile.read(reinterpret_cast<char*>(&gpa), sizeof(gpa));  
        inFile.read(reinterpret_cast<char*>(&courseLoadHours), sizeof(courseLoadHours));  
    }
    
};

class Employee : public CommunityMember {
protected:
    double salary;

public:
    Employee(int memberId = 0, string firstName = "", string lastName = "", string memberAddress = "", string memberCellPhone = "", double salary1 = 0)
        : CommunityMember(memberId, firstName, lastName, memberAddress, memberCellPhone), salary(salary1) {}

    void setSalary(double salary1) { salary = salary1; }
    double getSalary() const { return salary; }

    void operator++() { salary += salary * 0.05; }
    void operator--() { salary -= salary * 0.05; }

    void readData() override {
        CommunityMember::readData();
        cout << "Enter Salary: ";
        cin >> salary;
    }

    void display() const  {
        CommunityMember::print();
        cout << "Salary: " << salary << endl;
    }
};

class Staff : public Employee {
private:
    string department;

public:
    Staff(int memberId = 0, string firstName = "", string lastName = "", string memberAddress = "", string memberCellPhone = "", double salary1 = 0, const string& department1 = "")
        : Employee(memberId, firstName, lastName, memberAddress, memberCellPhone, salary1), department(department1) {}

    void readData() override {
        Employee::readData();
        cout << "Enter Department: ";
        cin >> department;
    }

    void display() const {
        Employee::display();
        cout << "Department: " << department << endl;
    }
};

class Faculty : public Employee {
private:
    string specialty;
    string academicRank;

public:
    Faculty(int memberId = 0, string firstName = "", string lastName = "", string memberAddress = "", string memberCellPhone = "", double salary1 = 0, const string& specialty1 = "", const string& academicRank1 = "")
        : Employee(memberId, firstName, lastName, memberAddress, memberCellPhone, salary1), specialty(specialty1), academicRank(academicRank1) {}

    void readData() override {
        Employee::readData();
        cout << "Enter Specialty: ";
        cin >> specialty;
        cout << "Enter Academic Rank: ";
        cin >> academicRank;
    }

    void print() const override {
        Employee::display();
        cout << "Specialty: " << specialty << endl;
        cout << "Academic Rank: " << academicRank << endl;
    }

    void printToFile(ofstream& outFile) const override {
        Employee::printToFile(outFile);
        outFile.write(specialty.c_str(), specialty.size() + 1);
        outFile.write(academicRank.c_str(), academicRank.size() + 1);
    }

    void readFromFile(ifstream& inFile) override {
        Employee::readFromFile(inFile);
        char buffer[256];

        inFile.getline(buffer, sizeof(buffer));
        specialty = buffer;

        inFile.getline(buffer, sizeof(buffer));
        academicRank = buffer;
    }
};

class Administrator : public Faculty {
private:
    char position[11];

public:
  Administrator(unsigned long long id = 0, const char* fname = "", const char* lname = "", const char* address = "", unsigned long long cellPhone = 0, double salary = 0.0, const char* specialty = "", const char* academicRank = "", const char* position = "")
    : Faculty(id, string(fname), string(lname), string(address), to_string(cellPhone), salary, string(specialty), string(academicRank)) {
    strncpy(this->position, position, 10);
    this->position[10] = '\0'; // Ensure null termination
}

    const char* getPosition() const {
        return position;
    }

    void setPosition(const char* position) {
        strncpy(this->position, position, 10);
        this->position[10] = '\0';  // Ensure null termination
    }

    bool operator==(const Administrator& other) const {
        return this->id == other.id;
    }

    void readData() override {
        Faculty::readData();
        cout << "Enter position: ";
        cin.ignore();  // Clear the buffer before reading a string
        cin.getline(position, 11);  // Read position
    }

    void print() const override {
        Faculty::print();
        cout << "Position: " << position << endl;
    }

    void printToFile(ofstream& outFile) const override {
        Faculty::printToFile(outFile);
        outFile.write(reinterpret_cast<const char*>(position), sizeof(position));
    }

    void readFromFile(ifstream& inFile) override {
        Faculty::readFromFile(inFile);
        inFile.read(reinterpret_cast<char*>(position), sizeof(position));
    }
};

class Teacher : public Faculty {
private:
    unsigned int hoursPerWeek;

public:
    Teacher(unsigned long long id = 0, const char* fname = "", const char* lname = "", const char* address = "", unsigned long long cellPhone = 0, double salary = 0.0, const char* specialty = "", const char* academicRank = "", unsigned int hoursPerWeek = 0)
    : Faculty(id, string(fname), string(lname), string(address), to_string(cellPhone), salary, string(specialty), string(academicRank)), hoursPerWeek(hoursPerWeek) {}


    void setHoursPerWeek(unsigned int hoursPerWeek) {
        this->hoursPerWeek = hoursPerWeek;
    }

    bool operator==(const Teacher& other) const {
        return this->id == other.id;
    }

    void readData() override {
        Faculty::readData();
        cout << "Enter hours per week: ";
        cin >> hoursPerWeek;
    }

    void print() const override {
        Faculty::print();
        cout << "Hours per Week: " << hoursPerWeek << endl;
    }

    void printToFile(ofstream& outFile) const override {
        Faculty::printToFile(outFile);
        outFile.write(reinterpret_cast<const char*>(&hoursPerWeek), sizeof(hoursPerWeek));
    }

    void readFromFile(ifstream& inFile) override {
        Faculty::readFromFile(inFile);
        inFile.read(reinterpret_cast<char*>(&hoursPerWeek), sizeof(hoursPerWeek));
    }
};
int main() {
    vector<CommunityMember*> members;
    while (true) {
        int choice;
        cout << "1. Add Employee" << endl;
        cout << "2. Add Student" << endl;
        cout << "3. Add Teacher" << endl;
        cout << "4. Add Administrator" << endl;
        cout << "5. Add Staff" << endl;
        cout << "6. Add Faculty" << endl;
        cout << "7. Add Alumni" << endl; 
        cout << "9. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        if (choice == 7) {
            Alumnus* alumni = new Alumnus();
            alumni->readData();
            members.push_back(alumni);
        }
        else if (choice == 2) {
            Student* student = new Student();
            student->readData();
            members.push_back(student); 
        }
        else if (choice == 3) {
            Teacher* teacher = new Teacher();
            teacher->readData();
            members.push_back(teacher);
        }
        else if (choice == 4) {
            Administrator* admin = new Administrator();
            admin->readData();
            members.push_back(admin);
        }
        else if (choice == 5) {
            Staff* staff = new Staff();
            staff->readData();
            members.push_back(staff);
        }
        else if (choice == 6) {
            Faculty* faculty = new Faculty();
            faculty->readData();
            members.push_back(faculty);
        }
        else if (choice == 1) {
            Employee* employee = new Employee();
            employee->readData();
            members.push_back(employee);
        }
        else if (choice == 9) {
            cout << "Exiting program." << endl;
            break;
        }
        else {
            cout << "Invalid choice. Please try again." << endl;
        }
}
    for (auto member : members) {
        delete member;  
    }
    return 0;
}




