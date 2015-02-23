#include <iostream>
#include <cstring>
#include <fstream>
#include <ctime>
#include <cstdlib>

using namespace std;

void wordtoread(char*);
void hinttoread(char*,int);
int numberlines();
void hangman(char*);
void Figure(int, char**);


int main(int argc, char* argv[]){
	srand(time(0));
	int ans = 0;
	cout<<"\nWelcome!\nLet's play a game to die for :)\nTry to guess the word correctly, Mr. Stickman is counting on you!\n"<<endl;
	do{
		char temp[30];
		char temp2[100];
		wordtoread(temp);
		hangman(temp);

		cout<<"Want to play again? 1. Yes 2. No"<<endl;
		cin>>ans;
	}while(ans==1);
	return 0;
}

void wordtoread(char* temp){
	ifstream myfile;
	myfile.open("words.txt");
	int nlines = numberlines(), count=0;
	int position = rand()%nlines;
	if(myfile){
		while(!myfile.eof()){
			myfile >> temp;
			if(position == count){
				break;
			}
			count++;
		}
		myfile.close();
	}else{
		cout<<"The file doesn't exist!"<<endl;
	}
}


int numberlines(){
	int number = 0;
	ifstream myfile;
	myfile.open("words.txt");
	string line;
	if(myfile.is_open()){
		while(getline(myfile,line)){
			number++;
		}
		myfile.close();
	}
	return number;
}


void hangman(char* word){
	char** figure = new char*[12];
	for(int i=0; i<12;i++){
		figure[i] = new char[1];
	}
	for(int i=0; i<12;i++){
		for(int j=0;j<12;j++){
			figure[i][j] = ' ';
		}
	}
	int size = strlen(word);
	char* trystring = new char[size];
	for(int i=0; i<size; i++){
		trystring[i] = '_';
	}
	int faults = 0;
	bool ifwin;
	char* guess = new char[20];
	do{
		cout<<"[  ";
		for(int i=0; i<size;i++){
			cout<<trystring[i]<<" ";
		}
		cout<<" ]\nEnter your guess: ";
		cin>>guess;
		bool ifright = false, ifword = false;
		int messageflag = 0;
		if(strlen(guess)==1){

			for(int i=0; i<size;i++){
				if(word[i] == guess[0]){
					if(trystring[i] == word[i]){
						if(messageflag==0){
							cout<<"It's already in the word!"<<endl;
							messageflag++;
						}
					}else{
						trystring[i] = word[i];
						if(messageflag==0){
							cout<<"Your guess was correct!"<<endl;
							messageflag++;
						}
					}
					ifright=true;
				}
			}
			int checkup = 0;
			for(int i=0; i<size;i++){
				if(trystring[i] == '_')
					checkup++;
			}
			if(checkup==0){
				ifwin = true;
			}
		}//if length is 1
		else{
			if(strlen(guess)!=strlen(word)){
				cout<<"The word you entered is too short"<<endl;
				ifright=false;
				continue;
			}else{
				for(int i=0; i<size;i++){
					if(word[i]==guess[i]){
						ifword=true;
					}else{
						ifword=false;
						break;
					}
				}
				if(ifword){
					for(int i=0; i<size;i++){
						trystring[i] = guess[i];
					}
					ifright=true;
					ifwin=true;
				}else{
					ifright=false;
				}
			}
		}
		if(ifright){
			continue;
		}else{
			cout<<"Your guess wasn't correct!"<<endl;
			faults++;
			Figure(faults, figure);
		}
		if(faults==6){
			break;
		}
		guess = new char[20];
	}while(!ifwin);

	if(ifwin){
		cout<<"You won the game! You saved Mr. Stickman!"<<endl;
	}else{
		cout<<"Bye bye Mr.Stickman.\nThe correct answer was > ";
		for(int i=0; i<size;i++){
			cout<<word[i];
		}
		cout<<" <"<<endl;
	}
	delete trystring;
	delete guess;
}
	


void Figure(int tries, char** figure){
	if(tries==1){
		for(int i=0; i<12;i++){
			for(int j=0;j<12;j++){
				if(j==0 && i>0){
					figure[i][j] = '|';
				} else if (i==0 && j%2==1 && j<6){
					figure[i][j] = '_';
				}
			}
		}
	} else if(tries==2){
		for(int i=0;i<12;i++){
			for(int j=0;j<12;j++){
				if(i==1){
					if(j==5){
						figure[i][j]='|';
					}else if(j>3&&j<7){
						figure[i][j]='_';
					}
				} else if(i==2){
					if(j==3){
						figure[i][j]='/';
					}else if(j==7){
						figure[i][j]='\\';
					}
				}else if(i==3){
					if(j==3){
						figure[i][j]='\\';
					}else if(j==7){
						figure[i][j]='/';
					}else if(j==4||j==6){
						figure[i][j]='_';
					}
				}
			}
		}
	}else if(tries==3){
		for(int i=0;i<12;i++){
			for(int j=0;j<12;j++){
				if(i>3&&i<8){
					if(j==5){
						figure[i][j] = '|';
					}
				}
			}
		}	
	}else if(tries==4){
		for(int i=0;i<12;i++){
			for(int j=0;j<12;j++){
				if(i==5){
					if(j>2&&j<5){
						figure[i][j]='_';
					}else if(j>5&&j<8){
						figure[i][j] = '_';
					}
				}
			}
		}
	} else if(tries==5){
		for(int i=0;i<12;i++){
			for(int j=0;j<12;j++){
				if(i==8){
					if(j==4){
						figure[i][j]='/';
					}
				}else if(i==9){
					if(j==3){
						figure[i][j]='/';
					}
				}
			}
		}
	} else if(tries==6){
		for(int i=0;i<12;i++){
			for(int j=0;j<12;j++){
				if(i==8){
					if(j==6){
						figure[i][j]='\\';
					}
				}else if(i==9){
					if(j==7){
						figure[i][j]='\\';
					}
				}
			}
		}
	} 
	for(int i=0;i<12;i++){
		for(int j=0;j<8;j++){
			cout<<figure[i][j];
		}
		cout<<'\n';
	}

}
