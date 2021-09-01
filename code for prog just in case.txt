#include "stdafx.h"
#include <iostream>
#include <string>
#include <fstream>                       //files
#include <iomanip>                      //setw
using namespace std;
struct employee
{
	string Fname;
	string Lname;
	string mail;
	string address;
	string phone;
	string job;
	int ID;
	int age;
	double payrate;
	double workedhours;
	double tax;
	double gross;
	double emp_total;
	double net;
};
employee emp[10];						//array of struct holding 10 employees data
int emp_count = 0;						//counter of employees and used sometimes as index for the array
int emp_no_after_sorting;              //holding the index of the last added employee after sorting

void welcome();          //welcome message at the beginning
void verification();    //not anyone can access employees and see their data 
void menu();			//main menu of the program and other functions are called within it
void add();				//add another emp function
void add_another_emp();		//asks for adding another emp!
void view_all();			//print id and name of all employees
void print();              // print function with many options
void edit();				//edit a current emp
void erase();				//delete emp
void compute();				//contains secondary menu with many compute options
void receiveF();			//recover data from files
void sendF();				//send data to files
void personal_info(int);			//option for adding some personal info 
void freelancer_gross(double &F_gross);      //calculate gross salary for a free lancer , wont be saved in the array (call by refrence)
void emp_compute(int);						//it calculate the employee gross salary-tax-pay rate-worked hours-net salary ....etc
void sort(int);								//sort employees automatically after adding or editing in the array by descending order
double calc_tax(double);                     // calculate taxes based on the gross salary
double calc_workedhours(double);			//ceil or floor for the worked hours 

int main()
{
	menu();

	return 0;
}

void verification()
{
	string user, pw;
	int count = 0;          //if pw or user entered 3 times wrong the program will terminate

	do
	{
		cout << "User name: ";
		getline(cin, user);
		cout << "Password: ";
		getline(cin, pw);
		if (user != "admin")
		{
			cout << "You have entered an invalid username or password !" << endl;
			count++;
			if (count == 3)
			{
				cout << "Please contact IT for help !" << endl;
				exit(0);
			}
			system("pause");
		}
		else
		{
			if (pw != "admin")
			{
				cout << "You have entered an invalid username or password !" << endl;
				count++;
				if (count == 3)
				{
					cout << "Please contact IT for help !" << endl;
					exit(0);
				}
				system("pause");
			}
		}
		system("cls");
	} while (user != "admin" || pw != "admin");
	system("cls");
}
void welcome()
{
	for (int i = 0; i < 10; i++)
		cout << endl;
	system("color 03");                           // changing color to light blue
	cout << setw(72) << "Welcome to thebes payroll" << endl;    // setw to center the welcome msg
	cout << setw(48);
	for (int i = 0; i < 25; i++)
		cout << "_";								//underline the welcome msg
	for (int i = 0; i < 10; i++)
		cout << endl;
	system("pause");								// holding screen until user enters any key
	system("color 07");								// changing color back to white
	system("cls");									// clearing screen
}
void menu()
{
	verification();
	welcome();
	receiveF();
	char choice;       //choosing operation the user want to do!
	do
	{
		system("cls");
		cout << setw(95) << "You have " << emp_count << " Employees currently !" << endl;       //viewing how many employees hired !
		cout << setw(119) << "__________________________________" << endl;
		cout << "A- Add \n\n"
			"C- Compute\n\n"
			"P- Print \n\n"
			"E- Edit \n\n"
			"D- Delete \n\n"
			"Q- Quit\n"
			"__________________\n\n"
			"Enter your Choice: ";
		cin >> choice;
		while (cin.get() != '\n')                            //handle entering anything except a char! 
		{
			cout << "Error try again !" << endl;
			cin.clear();
			cin.ignore(100, '\n');
			cout << "__________________\n\n"
				"Enter your Choice: ";
			cin >> choice;
		}
		switch (choice)
		{
		case 'A':
		case 'a':
		{
			add();
			add_another_emp();
			break;
		}
		case 'C':
		case 'c':
			compute();
			break;
		case 'E':
		case 'e':
			edit();
			break;
		case 'P':
		case 'p':
			print();
			break;
		case 'D':
		case 'd':
			erase();
			break;
		case 'Q':
		case 'q':
		{
			system("cls");																//clearing screen
			system("color 03");                                                        //changing color to light blue
			cout << setw(76) << "Thank you for using our system *_*" << endl;          //exit msg
			exit(0);
			break;
		}
		default:
		{
			cout << "Error try again !" << endl;
			system("pause");
			break;
		}
		}

	} while (choice != 'q' && choice != 'Q');              //we know it will already terminate inside the switch but its looks better than infinite loop 
}
void add()
{

	system("cls");
	if (emp_count > 9)
	{
		cout << "Exceeded employees number delete any before you can add again !" << endl;
		system("pause");
	}
	else
	{
		int choice_1;              //choosing to compute salary or not!
	again10:
		cout << "First name: ";                         //getting employee first and last name
		getline(cin, emp[emp_count].Fname);
		for (int i = 0; i < emp[emp_count].Fname.length(); i++)     //checking if there's a space in the first name
		{
			if (emp[emp_count].Fname[i]==' ')
			{
				cout << "Error dont enter a space!" << endl;
				goto again10;
			}
		}
	again11:
		cout << "Last name: ";
		getline(cin, emp[emp_count].Lname);
		for (int i = 0; i < emp[emp_count].Lname.length(); i++)   //checking if there's a space in the last name
		{
			if (emp[emp_count].Lname[i] == ' ')
			{
				cout << "Error dont enter a space!" << endl;
				goto again11;
			}
		}
	again1:                                             //if didnt choose 1 or 2 will come here
		system("cls");
		cout << "Do you want to compute his salary ?\n"
			"1-Yes\n"
			"2-No\n";
		cin >> choice_1;
		while (cin.get() != '\n')         // handle entering anything except int
		{
			cout << "Error try again ! " << endl;
			cin.clear();
			cin.ignore(100, '\n');
			cout << "Do you want to compute his salary ?\n"
				"1-Yes\n"
				"2-No\n";
			cin >> choice_1;
		}
		if (choice_1 == 1)              // computing employee salary 
		{
			emp_compute(emp_count);
			emp[emp_count].ID = 5001 + emp_count;
			system("cls");
			cout << "Employee Added Successfully" << endl;
			system("pause");
		}
		else if (choice_1 == 2)  //choosing not to compute will set all values to 0
		{

			emp[emp_count].ID = 5001 + emp_count;
			emp[emp_count].gross = 0;
			emp[emp_count].tax = 0;
			emp[emp_count].net = 0;
			emp[emp_count].payrate = 0;
			emp[emp_count].workedhours = 0;
			emp[emp_count].emp_total = 0;
			system("cls");
			cout << "Employee Added Successfully" << endl;
			system("pause");

		}
		else
		{
			cout << "Error try again !" << endl;
			system("pause");
			goto again1;
		}
		personal_info(emp_count);             //asking to add his personal info 
		sort(emp_count);					//sorting the added emp in the array
		emp_count++;							//incrementing the number of employees
		sendF();							//sending the new array to a file
	}
}
void add_another_emp()
{
	int choice_2;        //choose adding another employee or no!
	do
	{
		system("cls");
		cout << "Do you want to add another employee ?\n"
			"1-Yes\n"
			"2-No\n";
		cin >> choice_2;
		while (cin.get() != '\n')        //handle entering anything except int !
		{
			cout << "Error try again ! " << endl;
			cin.clear();
			cin.ignore(100, '\n');
			cout << "Do you want to add another employee ?\n"
				"1-Yes\n"
				"2-No\n";
			cin >> choice_2;
		}
		if (choice_2 == 1)            //if yes going back to the main add function 
		{
			add();
		}
		else if (choice_2 == 2)      // if no clearing the screen and back to the main menu
		{
			system("cls");
		}
		else
		{
			cout << "Error try again !" << endl;
			system("pause");

		}
	} while (choice_2 != 2);      // will ask for adding employees until user chooses not to !
}
void view_all()  //print id and name of all employees
{
	system("cls");
	cout << "#\tID\tName" << endl;
	for (int i = 0; i < emp_count; i++)
	{
		cout << i + 1 << "\t" << emp[i].ID << "\t" << emp[i].Fname << " " << emp[i].Lname << endl;
	}
	cout << "-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~" << endl;
}
void print()
{

	if (emp_count == 0)             //msg if no employees available
	{
		system("cls");
		cout << "No employees to print " << endl;
		system("pause");
		system("cls");
	}
	else
	{
	try2:                          // come here if user didnt enter 1,2,3 or 4 at printing options
		system("cls");
		int choice;                    // choosing print type
		cout << "1- Print all emplyees\n"
			"2- print specific employee\n"
			"3- Print an employee personal data\n"
			"4- Main menu\n"
			"___________________\n"
			"Enter your choice: ";
		cin >> choice;
		while (cin.get() != '\n')         //handle entering anything except int
		{
			cout << "Error try again !" << endl;
			cin.clear();
			cin.ignore(100, '\n');
			cout << "___________________\n"
				"Enter your choice: ";
			cin >> choice;
		}
		if (choice == 1)
		{
			system("cls");
			cout << "#\tID\tName\t\t\tPay rate\tWorked hours\tTax\t\tGross\t\tNet\n" << endl;
			for (int i = 0; i < emp_count; i++)
			{
				for (int j = i; j <= i; j++)
				{

					if (emp[j].Fname.length() + emp[j].Lname.length() > 14)    // taking less spaces if name is long
					{
						cout << i + 1 << "\t" << emp[j].ID << "\t" << emp[j].Fname << " " << emp[j].Lname << "\t" << emp[j].payrate << "\t\t"
							<< emp[j].workedhours << "\t\t" << emp[j].tax << "\t\t" << emp[j].gross << "\t\t" << emp[j].net << endl;
					}
					else if (emp[j].Fname.length() + emp[j].Lname.length() < 7) //taking more spaces if name is short
					{
						cout << i + 1 << "\t" << emp[j].ID << "\t" << emp[j].Fname << " " << emp[j].Lname << "\t\t\t" << emp[j].payrate << "\t\t"
							<< emp[j].workedhours << "\t\t" << emp[j].tax << "\t\t" << emp[j].gross << "\t\t" << emp[j].net << endl;
					}
					else                    //normal spacing
					{
						cout << i + 1 << "\t" << emp[j].ID << "\t" << emp[j].Fname << " " << emp[j].Lname << "\t\t" << emp[j].payrate << "\t\t"
							<< emp[j].workedhours << "\t\t" << emp[j].tax << "\t\t" << emp[j].gross << "\t\t" << emp[j].net << endl;
					}

				}
				cout << endl;
			}
			system("pause");
			system("cls");
		}
		else if (choice == 2)
		{
		try3:                            // come here if entered a number for non excisting employee
			view_all();
			int x;					// choose emp to print 
			cout << "Select employee u want to print: ";
			cin >> x;
			while (cin.get() != '\n')   //handle entering anything except int
			{
				cout << "Error try again ! " << endl;
				cin.clear();
				cin.ignore(100, '\n');
				cout << "Select employee u want to print: ";
				cin >> x;
			}
			if (x > emp_count || x <= 0)
			{
				cout << "Error! Out of range, try again" << endl;
				system("pause");
				goto try3;
			}
			else
			{
				system("cls");
				cout << "ID\tName\t\t\tPay rate\tWorked hours\tTax\t\tGross\t\tNet\n" << endl;
				if (emp[x - 1].Fname.length() + emp[x - 1].Lname.length() > 14)            // taking less spaces if name is long
				{
					cout << emp[x - 1].ID << "\t" << emp[x - 1].Fname << " " << emp[x - 1].Lname << "\t" << emp[x - 1].payrate << "\t\t"
						<< emp[x - 1].workedhours << "\t\t" << emp[x - 1].tax << "\t\t" << emp[x - 1].gross << "\t\t" << emp[x - 1].net << endl;
				}
				else if (emp[x - 1].Fname.length() + emp[x - 1].Lname.length() < 7)           //taking more spaces if name is short
				{
					cout << emp[x - 1].ID << "\t" << emp[x - 1].Fname << " " << emp[x - 1].Lname << "\t\t\t" << emp[x - 1].payrate << "\t\t"
						<< emp[x - 1].workedhours << "\t\t" << emp[x - 1].tax << "\t\t" << emp[x - 1].gross << "\t\t" << emp[x - 1].net << endl;
				}
				else                                             //normal spacing
				{
					cout << emp[x - 1].ID << "\t" << emp[x - 1].Fname << " " << emp[x - 1].Lname << "\t\t" << emp[x - 1].payrate << "\t\t"
						<< emp[x - 1].workedhours << "\t\t" << emp[x - 1].tax << "\t\t" << emp[x - 1].gross << "\t\t" << emp[x - 1].net << endl;
				}
				system("pause");
				system("cls");
			}
		}
		else if (choice == 3)
		{
		try5:          // come here if entered a number for non excisting employee
			view_all();
			int x;					// choose emp to view his personal info 
			cout << "Select employee: ";
			cin >> x;
			while (cin.get() != '\n')        // handle entering anything except int
			{
				cout << "Error try again ! " << endl;
				cin.clear();
				cin.ignore(100, '\n');
				cout << "Select employee u want to delete: ";
				cin >> x;
			}
			if (x > emp_count || x <= 0)
			{
				cout << "Error! Out of range, try again" << endl;
				system("pause");
				goto try5;
			}
			else
			{
				system("cls");         //if all his personal data = 0 then he didnt add any personal info
				if ((emp[x - 1].phone == "0") && (emp[x - 1].address == "0") && (emp[x - 1].mail == "0") && (emp[x - 1].job == "0") && (emp[x - 1].age == 0))
				{
					cout << "No personal info to view !" << endl;
					system("pause");
				}
				else
				{
					cout << "Name: " << emp[x - 1].Fname << " " << emp[x - 1].Lname << endl;
					if (emp[x - 1].age != 0)
						cout << "Age: " << emp[x - 1].age << endl;
					if (emp[x - 1].job != "0")
						cout << "Job Describtion: " << emp[x - 1].job << endl;
					if (emp[x - 1].phone != "0")
						cout << "Phone: " << emp[x - 1].phone << endl;
					if (emp[x - 1].address != "0")
						cout << "Address: " << emp[x - 1].address << endl;
					if (emp[x - 1].mail != "0")
						cout << "E-mail: " << emp[x - 1].mail << endl;
					system("pause");
				}
			}

		}
		else if (choice == 4)
		{
			system("cls");                  //clearing screen and then go back to main menu
		}
		else
		{
			cout << "Error! try again " << endl;
			system("pause");
			goto try2;
		}
	}
}
void edit()
{
	if (emp_count == 0)             //msg if no employees available      
	{
		system("cls");
		cout << "No employees to Edit " << endl;
		system("pause");
		system("cls");
	}
	else
	{
	try2:             //come here if entered a number for non excisting employee
		system("cls");
		view_all();
		int x;					// choose emp to edit 
		cout << "Select employee u want to edit: ";
		cin >> x;
		while (cin.get() != '\n')              // handle entering anything except int
		{
			cout << "Error try again ! " << endl;
			cin.clear();
			cin.ignore(100, '\n');
			cout << "Select employee u want edit: ";
			cin >> x;
		}
		if (x > emp_count || x <= 0)
		{
			cout << "Error! Out of range, try again" << endl;
			system("pause");
			goto try2;
		}
		else
		{
			system("cls");
			cout << "First Name: ";							//taking new name
			getline(cin, emp[x - 1].Fname);
			cout << "Last Name: ";
			getline(cin, emp[x - 1].Lname);
			emp_compute(x - 1);	                         //calculating his new salary
			personal_info(x - 1);						//asking to add personal info
			cout << "Employee edited successfully" << endl;
			system("pause");
			sort(x - 1);				//sorting after editing 
			sendF();					//sending the new array to file
		}
	}
}
void erase()
{
	if (emp_count <= 0)                   //msg if no employees available  
	{
		system("cls");
		cout << "No employees to delete " << endl;
		system("pause");
		system("cls");
	}
	else
	{
	try1:										  //come here if entered a number for non excisting employee
		system("cls");
		view_all();
		int choice;                  //for confirmation on delete 
		int x;					// choose emp to delete 
		cout << "Select employee u want to delete: ";
		cin >> x;
		while (cin.get() != '\n')     //handle entering anything except int
		{
			cout << "Error try again ! " << endl;
			cin.clear();
			cin.ignore(100, '\n');
			cout << "Select employee u want to delete: ";
			cin >> x;
		}
		if (x > emp_count || x <= 0)
		{
			cout << "Error! Out of range, try again" << endl;
			system("pause");
			goto try1;
		}
		else
		{
			system("cls");
		again:                // come here if didnt choose 1 or 2 at delete confirmation
			cout << "Are you sure you want to delete " << emp[x - 1].Fname << " " << emp[x - 1].Lname << " ?\n"
				"1- Yes\n"
				"2- No\n";
			cin >> choice;
			while (cin.get() != '\n')   // handle entering anything except int
			{
				cout << "Error try again ! " << endl;
				cin.clear();
				cin.ignore(100, '\n');
				cout << "Are you sure you want to delete " << emp[x - 1].Fname << " " << emp[x - 1].Lname << " ?\n"
					"1- Yes\n"
					"2- No\n";
				cin >> choice;
			}
			if (choice == 1)
			{
				for (int i = (x - 1); i < emp_count; i++)
				{
					emp[i] = emp[i + 1];                        //moving employees in index by the same order to delete 
				}
				cout << "Employee deleted Successfully" << endl;
				emp_count--;                            //decementing employees number after delete
				system("pause");
				system("cls");
			}
			else if (choice == 2)
			{
				system("cls");         //clearing screen to back to main menu
			}
			else
			{
				cout << "Error! try again" << endl;
				goto again;
			}
			sendF();               //send the new array to file
		}
	}
}
void compute()
{
	system("cls");
	char choice_1;         //for choosing from compute operations

again4:                // come here if didnt choose a correct compute operation
	cout <<
		"T- Total" << setw(20) <<
		"G- Gross" << setw(20) <<
		"N- Net" << setw(20) <<
		"X- Max" << setw(20) <<
		"I- Min" << setw(20) <<
		"M- Main menu" << setw(20) << endl;
	cout << "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~\n"
		"Enter your Choice: ";
	cin >> choice_1;
	while (cin.get() != '\n')  //handle entering anything except a char
	{
		cout << "Error! try again" << endl;
		cin.clear();
		cin.ignore(100, '\n');
		cout << "~~~~~~~~~~~~~~~~~~~~\n"
			"Enter your Choice: ";
		cin >> choice_1;
	}
	switch (choice_1)
	{
	case 'T':
	case 't':
	{
		system("cls");
		double total = 0;            //total payroll
		for (int i = 0; i < emp_count; i++)
		{
			total += emp[i].gross;            //adding gross for all employees to the total
		}
		cout << "Total company payroll = " << total << endl;
		system("pause");
		system("cls");
		break;
	}
	case 'G':
	case 'g':
	{
		int choice_2;          // for choosing operation 
	again5:                   // come here if didnt choose a correct operation 1 or 2
		system("cls");
		cout << "1- Add a new employee and calculate its gross salary\n"
			"2- Calculate gross salary for a free lancer (wont be saved)" << endl;
		cin >> choice_2;
		while (cin.get() != '\n')      //handle entering anything except int
		{
			cout << "Error try again !" << endl;
			cin.clear();
			cin.ignore(100, '\n');
			cout << "1- Add a new employee and calculate his gross salary\n"
				"2- Calculate gross salary for a free lancer (wont be saved)" << endl;
			cin >> choice_2;
		}
		if (choice_2 == 1)
		{
			add();           //calling the add function to add then compute his salary to print it here
			cout << "Gross salary for " << emp[emp_no_after_sorting].Fname << " " << emp[emp_no_after_sorting].Lname << " = " << emp[emp_no_after_sorting].gross << endl;
			system("pause");
			system("cls");
		}
		else if (choice_2 == 2)
		{
			double F_gross;
			freelancer_gross(F_gross);     // calculating gross for a free lancer and the value would be calculated in the variable sent (by refrence)
			F_gross *= 1.52;        //adding incentives
			cout << "Gross = " << F_gross << endl;
			system("pause");
			system("cls");

		}
		else
		{
			cout << "Error! try again" << endl;
			system("pause");
			goto again5;
		}
		break;
	}
	case 'N':
	case 'n':
	{
		int choice_2;          // for choosing operation 
	again6:						 // come here if didnt choose a correct operation 1 or 2
		system("cls");
		cout << "1- Add a new employee and calculate his net salary\n"
			"2- Calculate net salary for a free lancer (wont be saved)" << endl;
		cin >> choice_2;
		while (cin.get() != '\n')                //handle entering anything except int
		{
			cout << "Error! try again" << endl;
			cin.clear();
			cin.ignore(100, '\n');
			cout << "1- Add a new employee and calculate his net salary\n"
				"2- Calculate net salary for a free lancer (wont be saved)" << endl;
			cin >> choice_2;
		}
		if (choice_2 == 1)
		{
			add();									  //calling the add function to add then compute his salary to print it here
			cout << "net salary for " << emp[emp_no_after_sorting].Fname << " " << emp[emp_no_after_sorting].Lname << " = " << emp[emp_no_after_sorting].net << endl;
			system("pause");
		}
		else if (choice_2 == 2)
		{

			system("cls");
			double F_gross;
			double F_tax;
			double F_net;
			freelancer_gross(F_gross); // calculating gross for a free lancer and the value would be calculated in the variable sent (by refrence)
			F_gross *= 1.52;      //adding incentives
			F_tax = calc_tax(F_gross); //free lancer taxes calculation 
			F_net = F_gross - F_tax;  //his net salary
			cout << "Net = " << F_net << endl;
			system("pause");

		}
		else
		{
			cout << "Error! try again" << endl;
			system("pause");
			goto again6;
		}
		break;
	}
	case 'X':
	case 'x':
	{
		system("cls");
		if (emp_count == 0)           //msg if no employees available  
		{
			cout << "No Employees Available" << endl;
		}
		else
		{
			double max = emp[emp_count - 1].net;       //assigning array last net value to that variable  
			int x;
			for (int i = emp_count - 2; i >= 0; i--)
			{
				if (emp[i].net > max)                     //checking at the array if any net value is bigger than the max it will be assigned to it 
				{
					max = emp[i].net;
					x = i;
				}
			}
			cout << "Max Salary= " << max << " Payed For: " << emp[x].Fname << " " << emp[x].Lname << endl;
		}
		system("pause");
		system("cls");
		break;

	}
	case 'I':
	case 'i':
	{
		system("cls");
		if (emp_count == 0)               //msg if no employees available  
		{
			cout << "No Employees Available" << endl;
		}
		else
		{
			double min = emp[0].net;     //assigning array first net value to that variable 
			int z;
			for (int i = 1; i < emp_count; i++)
			{
				if (min > emp[i].net)           //checking at the array if any net value is smaller than the min it will be assigned to it 
				{
					min = emp[i].net;
					z = i;
				}
			}
			cout << "Min Salary= " << min << " Payed For: " << emp[z].Fname << " " << emp[z].Lname << endl;
		}
		system("pause");
		system("cls");
		break;
	}
	case 'M':
	case 'm':
	{
		system("pause");
		system("cls");
		break;
	}
	default:
	{
		cout << "Error! try again" << endl;
		system("pause");
		goto again4;
		break;
	}
	}
}
void receiveF()
{

	ifstream file;
	file.open("thebes.txt");
	file >> emp_count;        //receiving the number of emplyees first
	for (int i = 0; i < emp_count; i++)      //loop using number of emplyees to recover all their data
	{	
		file >> emp[i].Fname;
		file >> emp[i].Lname;
		file >> emp[i].job;
		file >> emp[i].phone;
		file >> emp[i].address;
		file >> emp[i].mail;
		file >> emp[i].age;
		file >> emp[i].ID;
		file >> emp[i].payrate;
		file >> emp[i].workedhours;
		file >> emp[i].tax;
		file >> emp[i].gross;
		file >> emp[i].net;
	}
	file.close();
}
void sendF()
{
	ofstream file;
	file.open("thebes.txt");
	file << emp_count << endl;  //send number of emplyees first
	for (int i = 0; i < emp_count; i++)  // sending all employees data to file one by one
	{
		file << emp[i].Fname << endl;
		file << emp[i].Lname << endl;
		file << emp[i].job << endl;
		file << emp[i].phone << endl;
		file << emp[i].address << endl;
		file << emp[i].mail << endl;
		file << emp[i].age << endl;
		file << emp[i].ID << endl;
		file << emp[i].payrate << endl;
		file << emp[i].workedhours << endl;
		file << emp[i].tax << endl;
		file << emp[i].gross << endl;
		file << emp[i].net << endl;
	}
	file.close();
}
void personal_info(int u) //u is the index of the employee
{
again5:							//come here if didnt choose 1 or 2 when adding personal info !
	system("cls");
	int choice;     //for choosing to add personal info or no
	cout << "Do you want to add personal info ?\n"
		"1- Yes\n"
		"2- No\n";
	cin >> choice;
	while (cin.get() != '\n') // handle entering anything except int 
	{
		cout << "Error try again !" << endl;
		cin.clear();
		cin.ignore(100, '\n');
		cout << "Do you want to add personal info ?\n"
			"1- Yes\n"
			"2- No\n";
		cin >> choice;
	}
	if (choice == 1)  // adding personal info !
	{
		cout << "Age: ";
		cin >> emp[u].age;
		while (cin.get() != '\n')     // handle entering anything except int
		{
			cout << "Error try again !" << endl;
			cin.clear();
			cin.ignore(100, '\n');
			cout << "Age: ";
			cin >> emp[u].age;
		}
	again12:
		cout << "Job Describtion: ";
		getline(cin, emp[u].job);
		for (int i = 0; i < emp[u].job.length(); i++)     //checking if there's a space in the job describtion
		{
			if (emp[u].job[i] == ' ')
			{
				cout << "Error dont enter a space!" << endl;
				goto again12;
			}
		}
	again7: //come here if entered anything except int numbers in the phone string
		cout << "Phone: ";
		getline(cin, emp[u].phone);
		for (int i = 0; i < emp[u].phone.length(); i++)
		{
			if (isalpha(emp[u].phone[i]) || emp[u].phone[i] == ' ')
			{
				cout << "Error enter numbers only !" << endl;
				goto again7;
			}
		}
	again13:
		cout << "Address: ";
		getline(cin, emp[u].address);
		for (int i = 0; i < emp[u].address.length(); i++)     //checking if there's a space in the address
		{
			if (emp[u].address[i] == ' ')
			{
				cout << "Error dont enter a space!" << endl;
				goto again13;
			}
		}
	again14:
		cout << "E-mail: ";
		getline(cin, emp[u].mail);
		for (int i = 0; i < emp[u].mail.length(); i++)     //checking if there's a space in the e-mail
		{
			if (emp[u].mail[i] == ' ')
			{
				cout << "Error dont enter a space!" << endl;
				goto again14;
			}
		}
	}
	else if (choice == 2)      //assigning all personal info to 0 if choose not to add them
	{
		emp[u].age = 0;
		emp[u].job = "0";
		emp[u].phone = "0";
		emp[u].address = "0";
		emp[u].mail = "0";
	}
	else
	{
		cout << "Error try again !" << endl;
		system("pause");
		goto again5;
	}
}
void freelancer_gross(double &F_gross)  // return gross by refrence
{
	system("cls");
	double hours = 0;    //hours worked before processing (ceil or floor)
	int choice;      // choose the way user want to enter hours
	double pr;    // pay rate
	double whours;  // worked hours
gg1:
	cout << "Enter employee pay rate: ";
	cin >> pr;
	while (cin.get() != '\n')      //handle entering any chars
	{
		cout << "Error try again ! " << endl;
		cin.clear();
		cin.ignore(100, '\n');
		goto gg1;
	}
	if (pr < 0)  // handle entering a negative number
	{
		cout << "Error try again ! " << endl;
		goto gg1;
	}
again6:         // come here if didnt choose 1 or 2 (way of entering hours worked)
	cout << "1- Enter hours worked per month\n"
		"2- Enter daily hours worked\n";
	cin >> choice;
	while (cin.get() != '\n')  // handle entering anything except int 
	{
		cout << "Error try again ! " << endl;
		cin.clear();
		cin.ignore(100, '\n');
		cout << "1- Enter hours worked per month\n"
			"2- Enter daily hours worked\n";
		cin >> choice;
	}
	if (choice == 1)
	{
	gg5:
		cout << "Hours worked this month: ";
		cin >> hours;
		while (cin.get() != '\n')  // handle entering a char
		{
			cout << "Error try again ! " << endl;
			cin.clear();
			cin.ignore(100, '\n');
			goto gg5;
		}
		if (hours > 372 || hours < 0) // handle entering a negative number or hours more than any employee can work
		{
			cout << "Error Exceeded monthly hours limit !" << endl;
			goto gg5;
		}
	}
	else if (choice == 2)
	{
		int n;           //days worked
		double h;            //hours per day
	gg3:
		cout << "How many days worked this month: ";
		cin >> n;
		while (cin.get() != '\n') //handle entering a char
		{
			cout << "Error try again ! " << endl;
			cin.clear();
			cin.ignore(100, '\n');
			goto gg3;
		}
		if (n > 31 || n < 0) // handle entering a negative number or days more than month days
		{
			cout << "Error Exceeded month days limit ! " << endl;
			goto gg3;
		}
		for (int i = 1; i <= n; i++)
		{
		gg2:
			cout << "day " << i << " worked hours: ";
			cin >> h;
			while (cin.get() != '\n')   //handle entering a char
			{
				cout << "Error try again ! " << endl;
				cin.clear();
				cin.ignore(100, '\n');
				goto gg2;
			}
			if (h > 12 || h < 0)     // handle entering a negative number or hours more than any employee can work daily
			{
				cout << "Error Exceeded daily hours limit !" << endl;
				goto gg2;
			}
			hours += h;
		}
	}
	else
	{
		cout << "Error try again ! " << endl;
		system("pause");
		goto again6;
	}
	whours = calc_workedhours(hours);
	F_gross = pr * whours;
}
void emp_compute(int l)  //l is the index of the employee in the array
{

	int choice;
gg4:
	cout << "Enter employee pay rate: ";
	cin >> emp[l].payrate;
	while (cin.get() != '\n')  // handle entering any char
	{
		cout << "Error try again ! " << endl;
		cin.clear();
		cin.ignore(100, '\n');
		goto gg4;
	}
	if (emp[l].payrate < 0)     // handle entering any negative number
	{
		cout << "Error try again ! " << endl;
		goto gg4;
	}
again3:     // come here if didnt choose 1 or 2
	cout << "1- Enter hours worked per month\n"
		"2- Enter daily hours worked\n";
	cin >> choice;
	while (cin.get() != '\n')    // handle entering a char
	{
		cout << "Error try again ! " << endl;
		cin.clear();
		cin.ignore(100, '\n');
		cout << "1- Enter hours worked per month\n"
			"2- Enter daily hours worked\n";
		cin >> choice;
	}
	double hours = 0;    //hours worked before processing (ceil or floor)
	if (choice == 1)
	{
	gg6:
		cout << "Hours worked this month: ";
		cin >> hours;
		while (cin.get() != '\n')       //handle entering a char
		{
			cout << "Error try again ! " << endl;
			cin.clear();
			cin.ignore(100, '\n');
			goto gg6;
		}
		if (hours > 372 || hours < 0)      //handle entering a negative number or exceeded monthly hours limit
		{
			cout << "Error! Exceeded monthly hours limit" << endl;
			goto gg6;
		}

	}
	else if (choice == 2)
	{
		int n;           //days worked
		double h;            //hours per day
	gg7:
		cout << "How many days worked this month: ";
		cin >> n;
		while (cin.get() != '\n')      //handle entering a char
		{
			cout << "Error try again ! " << endl;
			cin.clear();
			cin.ignore(100, '\n');
			goto gg7;
		}
		if (n > 31 || n < 0)    //handle entering a negative number or exceeded monthly days limit
		{
			cout << "Error! Exceeded month days limit " << endl;
			goto gg7;
		}
		for (int i = 1; i <= n; i++)
		{
		gg8:
			cout << "day " << i << " worked hours: ";
			cin >> h;
			while (cin.get() != '\n')   //handle entering a char
			{
				cout << "Error try again ! " << endl;
				cin.clear();
				cin.ignore(100, '\n');
				goto gg8;
			}
			while (h > 12 || h < 0)   //handle entering a negative number or exceeded daily hours limit
			{
				cout << "Error! Exceeded daily hours limit " << endl;
				goto gg8;
			}

			hours += h;     //adding all daily hours here
		}

	}
	else
	{
		cout << "Error! try again" << endl;
		system("pause");
		goto again3;
	}
	/*doing calculations now on gross ,tax and net*/
	emp[l].workedhours = calc_workedhours(hours);
	emp[l].emp_total = emp[l].payrate * 176;
	if (emp[l].workedhours > 176)
	{
		emp[l].gross = (emp[l].emp_total + (emp[l].payrate*1.5*(emp[l].workedhours - 176)))* 1.52;
	}
	else if (emp[l].workedhours < 176)
	{
		emp[l].gross = (emp[l].emp_total - ((176 - emp[l].workedhours)*1.25*emp[l].payrate))* 1.52;
	}
	else
		emp[l].gross = emp[l].emp_total* 1.52;
	emp[l].tax = calc_tax(emp[l].gross);
	emp[l].net = emp[l].gross - emp[l].tax;
}
void sort(int y) //y is the index of employee sent to the function
{
	/*this part is for checking all the employees added before him and place him correctly based on his net salary in descending order
	and move the other to free a space for him before adding him in the right spot */
	for (int i = 0; i < y; i++)
	{
		if (emp[y].net > emp[i].net)
		{

			employee temp = emp[y];
			for (int j = y; j > i; j--)
			{
				emp[j] = emp[j - 1];
			}
			emp[i] = temp;

			emp_no_after_sorting = i;  //his index after sorting 
			break;
		}
		else
			emp_no_after_sorting = emp_count;  //his index if not sorted

	}
	/*this part is used after edit if his salary is reduced checking all the employees after him and place him correctly based on his net salary in descending order
	and move the other to free a space for him before adding him in the right spot */
	for (int i = emp_count - 1; i > y; i--)
	{
		if (emp[y].net < emp[i].net)
		{
			employee temp = emp[y];
			for (int j = y; j < i; j++)
			{
				emp[j] = emp[j + 1];
			}
			emp[i] = temp;

			emp_no_after_sorting = i;     //his index after sorting 
			break;
		}
		else
			emp_no_after_sorting = y;   //his index if not sorted
	}
}
double calc_tax(double k)  //k is gross salary
{
	double tax;           //calculating tax based on the gross salary then return it 
	if (k > 0 && k < 15000.00)
	{
		tax = 0.15 * k;
	}
	else if (k >= 15000.00 && k < 30000.00)
	{
		tax = 0.16 * (k - 15000.00) + 2250.00;
	}
	else if (k >= 30000.00 && k < 50000.00)
	{
		tax = 0.18 * (k - 30000.00) + 4650.00;
	}
	else if (k >= 50000.00 && k < 80000.00)
	{
		tax = 0.20 * (k - 50000.00) + 8250.00;
	}
	else if (k >= 80000.00 && k < 150000.00)
	{
		tax = 0.25 * (k - 80000.00) + 14250.00;
	}
	else
		tax = 0.00;

	return tax;
}
double calc_workedhours(double y) //y is the hours before processing
{
	double x = y - int(y);  // x holding the the float after decimal point 
	double z;          //worked hours after processing !
	if (x >= 0.5)
		z = ceil(y);
	else
		z = floor(y);
	return z;
}