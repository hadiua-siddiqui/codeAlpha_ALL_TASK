//TASK 1 "NUMBER GUESSING GAME
#include <iostream>
#include <cstdlib>
#include <ctime>
using namespace std;

int main()
{
    int num, guess, tries = 0;
    srand(time(0)); 
    num = rand() % 100 + 1; 
    cout << "Welcome To Numbere Guessing Game\n\n";

    do
    {
        cout << "Guess a number between 1 and 100 : ";
        cin >> guess;
        tries++;

        if (guess > num)
            cout << "Too high!\n\n";
        else if (guess < num)
            cout << "Too low!\n\n";
        else
            cout << "\n YOU GUESS THE CORRECT NUMBER  " << tries << " guesses!\n";
    } while (guess!= num);

    return 0;
}
//TASK 2 "BASIC FILE MANGER"
//BASIC FILE MANAGER
#include<iostream>
#include<fstream>
#include<string>
#include<cstdlib>

using namespace std;
void listFiles(const string& path) {
	ifstream dir(path.c_str());
	if(!dir){
		cerr<<"ERROR: UNABLE TO OPEN DIRECTORY."<<endl;
		return;
			}
			string item;
			while(getline(dir, item)){
				cout<<item<<endl;
			}
			dir.close();
}
void copyFile(const string& source, const string& destination){
	ifstream sourceFile(source.c_str(),ios::binary);
	ofstream destFile(destination.c_str(),ios::binary);
	destFile<< sourceFile.rdbuf();
	sourceFile.close();
	destFile.close();
	cout<<"File copied successfully." <<endl;
}

void moveFile(const string&source, const string& destination){
	if(rename(source.c_str(),destination.c_str())==0)
	cout<<"file moved successfully"<<endl;
	else
	cerr<<"Erro:unable  to move file."<<endl;
}

void viewFile(const string & filename){
	ifstream file(filename.c_str());
	if(file.is_open()) {
		string line;
		while (getline(file, line)){
			cout<<line<<endl;
		}
		file.close();
	}else {
		cerr<<"Erro:unable  to move file."<<endl;
	}
}

int main() {
	string command;
	string path= ".";
	while(true){
		cout<<"CURRENT DIRECTORY :"<<path<<endl;
		cout<<"enter command(list,creat,copy,view,move,exit):";
		cin>>command;
		if(command=="list"){
			listFiles(path);
		}
		else if(command=="create"){
			string dirname;
			cout<<"enter directory name:";
			cin>>dirname;
		}
		else if(command=="copy"){
			string source, destination;
			cout<<"enter source file path:";
			cin>>source;
			cout<<"Enter destination file path:";
			cin>>destination;
			copyFile(source, destination);
		}
		else if(command=="move"){
			string source,destination;
			cout<<"enter source file path:";
			cin>>source;
			cout<<"Enter destination file path:";
			cin>>destination;
			moveFile(source, destination);
			
		}else if(command=="view"){
			string filename;
		  cout<<"enter a filename";
		  cin>>filename;
		  viewFile(path+"/"+ filename);
		}
		else if (command=="exit"){
			break;
		}else{
			cerr<<"INVALID COMMAND.PLEASE TRY AGAIN."<<endl;
		
		}
	}
	return 0;
}

//TASK 3  "SODUKO PUZZLE SOLVING"

#include <iostream>
// N is the size of the 2D matrix N*N
#define H 9
using namespace std;
/* A utility function to print grid */
void print(int arr[H][H])
{
	for (int i = 0; i < H; i++) 
	{
		for (int j = 0; j < H; j++)
			cout << arr[i][j] << " ";
		cout << endl;
	}
}
bool isSafe(int grid[H][H], int row, 
					int col, int num)
{
	
	for (int x = 0; x <= 8; x++)
		if (grid[row][x] == num)
			return false;
	for (int x = 0; x <= 8; x++)
		if (grid[x][col] == num)
			return false;
	int startRow = row - row % 3, 
			startCol = col - col % 3;

	for (int i = 0; i < 3; i++)
		for (int j = 0; j < 3; j++)
			if (grid[i + startRow][j + 
							startCol] == num)
				return false;

	return true;
}

bool solvingSudoku(int grid[H][H], int row, int col)
{
	
	if (row == H - 1 && col == H)
		return true;

	if (col == H) {
		row++;
		col = 0;
	}

	if (grid[row][col] > 0)
		return solvingSudoku(grid, row, col + 1);

	for (int num = 1; num <= H; num++) 
	{

		if (isSafe(grid, row, col, num)) 
		{
			
			grid[row][col] = num;
		
			if (solvingSudoku(grid, row, col + 1))
				return true;
		}
	
		grid[row][col] = 0;
	}
	return false;
}
int main()
{
	// 0 means unassigned cells
	int grid[H][H] = { { 3, 0, 6, 5, 0, 8, 4, 0, 0 },
					{ 5, 2, 0, 0, 0, 0, 0, 0, 0 },
					{ 0, 8, 7, 0, 0, 0, 0, 3, 1 },
					{ 0, 0, 3, 0, 1, 0, 0, 8, 0 },
					{ 9, 0, 0, 8, 6, 3, 0, 0, 5 },
					{ 0, 5, 0, 0, 9, 0, 6, 0, 0 },
					{ 1, 3, 0, 0, 0, 0, 2, 5, 0 },
					{ 0, 0, 0, 0, 0, 0, 0, 7, 4 },
					{ 0, 0, 5, 2, 0, 6, 3, 0, 0 } };

	if (solvingSudoku(grid, 0, 0))
		print(grid);
	else
		cout << "SODUKO SOLUTION IS NOT FOUND " << endl;

	return 0;
}
