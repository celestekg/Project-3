# Project-3: This program calculates the fare for a parking lot. The time entered and exited is entered by the user, and the program calculates the appropriate payment due. The program also accounts for circumstances such as a lost ticket or a parking pass. The program then prints out a reciept listing the times entered and exited, and the fare due.

![image](https://user-images.githubusercontent.com/56613527/162595335-a66a6f86-6448-4af1-9e43-caf0050f2a86.png)

#include <iostream>
#include <iomanip>
#include <string>

using namespace std;

const int MINUTES_PER_HOUR = 60;
const int LOST_TICKET = 55;
const int SPECIAL_PARKING_PASS = 99;
const double ERROR_IN_TIME = -1.0;

void readingArrivalDepartureTimes(int& entranceHour, int& entranceMinute, int& exitHour, int& exitMinute);
int calculateTotalTime(int entranceHour, int entranceMinute, int exitHour, int exitMinute);
double determinePay(int totalTime);
void printReceipt(int entranceHour, int entranceMinute, int exitHour, int exitMinute, int totalTime, double pay);

int main()
{
	int entranceHour;
	int entranceMinute;
	int exitHour;
	int exitMinute;
	int totalTime;
	double pay;

	readingArrivalDepartureTimes(entranceHour, entranceMinute, exitHour, exitMinute);

	totalTime = calculateTotalTime(entranceHour, entranceMinute, exitHour, exitMinute);

	pay = determinePay(totalTime);

	printReceipt(entranceHour, entranceMinute, exitHour, exitMinute, totalTime, pay);

	return 0;
}

void readingArrivalDepartureTimes(int& entranceHour, int& entranceMinute, int& exitHour, int& exitMinute)
{
	cout << "Enter the Entrance Hour (0-23):   ";
	cin >> entranceHour;

	cout << "Enter the Entrance Minute (0-59): ";
	cin >> entranceMinute;

	cout << "Enter the Exit Hour (0-23):       ";
	cin >> exitHour;

	cout << "Enter the Exit Minute (0-59):     ";
	cin >> exitMinute;
}

int calculateTotalTime(int entranceHour, int entranceMinute, int exitHour, int exitMinute)
{
	if (entranceHour == SPECIAL_PARKING_PASS && entranceMinute == SPECIAL_PARKING_PASS)
		return entranceHour;
	else if (exitHour == LOST_TICKET || exitMinute == LOST_TICKET)
		return LOST_TICKET;
	else
	{
		if (exitHour < entranceHour)
			exitHour += 24;

		return (exitHour * MINUTES_PER_HOUR + exitMinute) - (entranceHour * MINUTES_PER_HOUR + entranceMinute);
	}
}

double determinePay(int totalTime)
{
	if (totalTime == SPECIAL_PARKING_PASS)
		return 5.0;
	else if (totalTime == LOST_TICKET)
		return 110.0;
	else if (totalTime < 30)
		return 3.0;
	else if (totalTime <= 60)
		return 5.0;
	else if (totalTime <= 120)
		return 10.0;
	else if (totalTime <= 180)
		return 15.0;
	else if (totalTime <= 240)
		return 30.0;
	else if (totalTime <= 720)
	{
		double price = 30;
		while (totalTime >= 240)
		{
			price += 5.00;
			totalTime -= 30;
		}

		return price;
	}
	else
		return ERROR_IN_TIME;
}

void printReceipt(int entranceHour, int entranceMinute, int exitHour, int exitMinute, int totalTime, double pay)
{
	cout << "\nReceipt..." << endl;
	cout << "Entrance Hour:   " << entranceHour << endl;
	cout << "Entrance Minute: " << entranceMinute << endl;
	cout << "Exit Hour:       " << exitHour << endl;
	cout << "Exit Minute:     " << exitMinute << endl;

	if (totalTime == -1.0)
		cout << "Error: Too Many Hours" << endl;
	else
	{
		if (entranceHour == SPECIAL_PARKING_PASS)
			cout << " Special Parking Pass" << endl;
		else if (exitHour == LOST_TICKET)
			cout << " Lost Ticket" << endl;
		else if (totalTime == ERROR_IN_TIME)
			cout << " Error: Too Many Hours" << endl;
		else
			cout << "Time in Deck:    " << totalTime / MINUTES_PER_HOUR << " Hour(s) and " << totalTime % MINUTES_PER_HOUR << " Minute(s)" << endl;

		cout << "Rate in Dollars: " << setprecision(2) << fixed << pay << endl;
	}
	cout << endl;
}
