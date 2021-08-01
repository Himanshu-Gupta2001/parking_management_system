/*
  Welcome to our c++ project it is a car parking software
  In this project we have used classes, switch and file handling.
*/

#include<fstream.h>  //file I/O and cout and cin
#include<conio.h>    //for clrscr() and getch()
#include<string.h>   //for string operations
#include<stdio.h>    //for gets() and puts()
#include<dos.h>      //for delay() function
#include<stdlib.h>   //itoa() integer to string

const int parkingDelay = 500;
fstream file;

void line()
{
  cout<<endl;
  for(int i=0;i<80;i++)
  {
    cout<<"*";
    delay(5);
  }
  cout<<endl;
}

class ParkingManagement
{
  unsigned long amount;
  int booked,rickshaw,car,bus,vehicles;
  int maxvehicles;
  char cnumbers[50][7];

  public:

  ParkingManagement()
  {
    maxvehicles = 50;
    booked = amount = car = bus = rickshaw = vehicles = 0;
  }

  void Park();
  void ParkCar();
  void ParkBus();
  void ParkRickshaw();
  void ShowRecord();
  void DeleteRecord();
  void ModifyRecord();
  void SaveRecord();
};

void ParkingManagement::Park()
{
  clrscr();
  line();

  if(vehicles >= maxvehicles)
  {
    cout<<"Parking full!\n";
    getch();
    return;
  }

  cout<<"Enter the vehicle type :\n";
  cout<<"Select the type :\n";
  cout<<"1. Car\n2. Bus\n3. Rickshaw\n";
  char ch = getch()-'0';

  if(ch == 1)
    ParkCar();
  else
  if(ch == 2)
    ParkBus();
  else
  if(ch == 3)
    ParkRickshaw();

  line();
  getch();
}

void ParkingManagement::ParkCar()
{
  cout<<"Amount added = 200\n";
  delay(parkingDelay);
  cout<<"Your coupon number is : C"<<car<<" (Remember your coupon number!)\n";
  cout<<"Car parked!\n";

  amount += 200;
  char code[7];
  strcpy(code,"C");
  char temp[10];
  itoa(car,temp,10);
  strcat(code,temp);
  strcpy(cnumbers[vehicles],code);
  vehicles++;
  car++;
}

void ParkingManagement::ParkBus()
{
  cout<<"Amount added = 300\n";
  delay(parkingDelay);
  cout<<"Your coupon number is : B"<<bus<<" (Remember your coupon number!)\n";
  cout<<"Bus parked!\n";

  amount += 300;
  char code[7];
  strcpy(code,"B");
  char temp[10];
  itoa(bus,temp,10);
  strcat(code,temp);
  strcpy(cnumbers[vehicles],code);
  vehicles++;
  bus++;
}

void ParkingManagement::ParkRickshaw()
{
  cout<<"Amount added = 100\n";
  delay(parkingDelay);
  cout<<"Your coupon number is : R"<<rickshaw<<" (Remember your coupon number!)\n";
  cout<<"Rickshaw parked!\n";

  amount += 100;
  char code[7];
  strcpy(code,"R");
  char temp[10];
  itoa(rickshaw,temp,10);
  strcat(code,temp);
  strcpy(cnumbers[vehicles],code);
  vehicles++;
  rickshaw++;
}

void ParkingManagement::ShowRecord()
{
  clrscr();
  line();

  char choice;
  cout<<"1. To read record of Cars\n"
      <<"2. To read record of Rickshaws\n"
      <<"3. To read record of Buses\n"
      <<"4. To read all the record\n";

  choice = getch() - '0';
  cout<<endl;
  if(choice == 1)
  {
    cout<<"The total number of car = "<<car<<endl;
    cout<<"Total amount collected from cars = "<<car*200<<endl;
  }
  else
  if(choice == 2)
  {
    cout<<"The total number of rickshaws = "<<rickshaw<<endl;
    cout<<"Total amount collected from rickshaws = "<<rickshaw*100<<endl;
  }
  else
  if(choice == 3)
  {
    cout<<"The total number of bus = "<<bus<<endl;
    cout<<"Total amount collected from buses = "<<bus*300<<endl;
  }
  else
  if(choice == 4)
  {
    cout<<"The total number of car = "<<car<<endl;
    cout<<"Total amount collected from cars = "<<car*200<<endl;
    cout<<"The total number of rickshaws = "<<rickshaw<<endl;
    cout<<"Total amount collected from rickshaws = "<<rickshaw*100<<endl;
    cout<<"The total number of bus = "<<bus<<endl;
    cout<<"Total amount collected from buses = "<<bus*300<<endl;
    cout<<"Total vehicles = "<<vehicles<<endl;
    cout<<"Total amount = "<<amount<<endl;
  }

  line();
  getch();
}

void ParkingManagement::DeleteRecord()
{
  clrscr();
  line();
  remove("Parking.dat");

  car = rickshaw = bus = 0;
  amount = 0;
  vehicles = 0;

  cout<<"All records deleted!\n";

  line();
  getch();
}

void ParkingManagement::ModifyRecord()
{
  clrscr();
  line();

  char cn[10];
  int found = 0;
  cout<<"Please enter your coupon number : ";
  gets(cn);

  for(int i=0;i<vehicles;i++)
  {
    if(strcmp(cn,cnumbers[i]) == 0)
    {
      char vname[10];
      found = 1;
      vehicles--;

      strcpy(vname,((cn[0] == 'C')?"Car":
	((cn[0] == 'B')?"Bus":
	  "Rickshaw")));

      cout<<"Your "<<vname<<" is found!\n";
      cout<<"You can take your "<<vname<<"."<<endl;

      for(int j=i;j<vehicles;j++)
      {
	strcpy(cnumbers[j],cnumbers[j+1]);
      }

      if(cn[0] == 'C')
	car--;
      else
      if(cn[0] == 'B')
	bus--;
      else
      if(cn[0] == 'R')
	rickshaw--;

      cout<<"Record modified!\n";
    }
  }

  if(found == 0)
    cout<<"Invalid coupon number!\n";

  line();
  getch();
}

void ParkingManagement::SaveRecord()
{
  clrscr();
  line();

  file.open("Parking.dat",ios::binary|ios::out);
  file.write((char*)this,sizeof(this));
  file.close();
  file.clear();

  cout<<"Record saved to file!\n";
  line();
  getch();
}

void frontpage()
{
  clrscr();
  for(int x=10;x<=70;x++)
  {
    gotoxy(x,1);
    cout<<"*";

    gotoxy(x,25);
    cout<<"*";
  }

  for(int y=1;y<=25;y++)
  {
    gotoxy(10,y);
    cout<<"*";

    gotoxy(70,y);
    cout<<"*";
  }

  gotoxy(28,3);
  cout<<"RRK Senior Secondary School";
  gotoxy(36,4);
  cout<<"Chandausi";
  gotoxy(31,6);
  cout<<"Session : 2018-2019";

  gotoxy(36,11);
  cout<<"Welcome to";
  gotoxy(31,12);
  cout<<"Parking Management";

  gotoxy(15,19);
  cout<<"Submitted to";
  gotoxy(15,20);
  cout<<"Anirudh Sir";

  gotoxy(35,20);
  cout<<"Submitted by";
  gotoxy(35,21);
  cout<<"Chirag Agrawal and Himanshu Gupta";
  gotoxy(35,22);
  cout<<"XII A";
}

void main()
{
  textbackground(BLUE);
  textcolor(YELLOW);
  ParkingManagement p;

  file.open("Parking.dat",ios::binary|ios::in);
  file.read((char*)&p,sizeof(p));
  file.close();
  file.clear();

  _setcursortype(0);  //no cursor
  frontpage();
  getch();
  _setcursortype(2);  //normal cursor

  char choice;
  int Saved = 0,dataEdited = 0;

  do
  {
    clrscr();
    gotoxy(36,1);
    cout<<"Main Menu\n\n";
    cout<<"Select your choice :\n";
    cout<<"1. To show the record\n"
	<<"2. To park a vehicle\n"
	<<"3. To erase all the records\n"
	<<"4. To modify the record\n"
	<<"5. To save all records\n"
	<<"0. To exit (ESC)\n";

    choice = getch()-'0';
    if(choice == 1)
    {
      p.ShowRecord();
    }
    else
    if(choice == 2)
    {
      p.Park();
      dataEdited = 1;
      Saved = 0;
    }
    else
    if(choice == 3)
    {
      p.DeleteRecord();
    }
    else
    if(choice == 4)
    {
      p.ModifyRecord();
      dataEdited = 1;
      Saved = 0;
    }
    else
    if(choice == 5)
    {
      Saved = 1;
      line();

      file.open("Parking.dat",ios::binary|ios::out);
      file.write((char*)&p,sizeof(p));
      file.close();
      file.clear();

      cout<<"Record saved to file!\n";
      line();
      getch();
    }
    else
    if(Saved == 0 && dataEdited == 1)
    if(choice == 0 || choice == 27-'0')
    {
      cout<<"Please save before exit!\n";
      getch();
      choice = -1;
    }
  }while(!(choice == 0 || choice == 27-'0'));
}