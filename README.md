# Library-management-system
A professional console-based Library Management System developed in C++ using Object-Oriented Programming and file handling concepts. It efficiently manages student records and book inventory, supports secure book issue and return operations, tracks complete transaction history with date and time, and ensures persistent data storage through files.
#include <iostream>
#include <fstream>
#include <limits>
#include <ctime>
using namespace std;
class student{
protected:
    string sname,scourse,smo;
    int sroll;
   
    public:
    string currentDateTime(){
        time_t now=time(0);
        return ctime(&now);
    }
    void detail(){
        int num;
        cout<<"ENTER DETAIL STUDENT OF MEMBER NUMBER :";
        cin>>num;
        for(int i=1;i<=num;i++){
            cout<<"STUDENT="<<i<<endl;
        ofstream  s("C:\\Users\\dc\\Desktop\\projects.cpp\\studentdt.txt",ios::app);
        cout<<"ENTER NAME :";
        cin.ignore();
        getline(cin, sname);
        cout<<"ENTER COURSE :";
        getline(cin,scourse);
        cout<<"ENTER ROLL NUMBER :";
        cin>>sroll;
        cout<<"MOBAIL NUMBER: ";
       cin.ignore();
       getline(cin,smo);
          s<<sname<<"|"<<scourse<<"|"<<sroll<<"|"<<smo<<endl;
        cout<<"STUDENTE ADD SUCCESSFULL:"<<endl;
        }
        }
        void showstdetail(){
            bool found=false;

            cout<<"============ALL STUDENT=============\n";
            ifstream s("C:\\Users\\dc\\Desktop\\projects.cpp\\studentdt.txt");
            while(getline(s,sname,'|')){
                getline(s,scourse,'|');
                s>>sroll;s.ignore();
                getline(s,smo);
                cout<<"STUDENT NAME ="<<sname<<endl;
                cout<<"STUDENT COURSE="<<scourse<<endl;
                cout<<"STUDENT ROLL NUMBER ="<<sroll<<endl;
                cout<<"STUDENT MOBAILE NUMBER ="<<smo<<endl;
                cout<<"============================================\n";
            }
            s.close();
        }
        bool studentout(int roll,string &nameh){
         ifstream s("C:\\Users\\dc\\Desktop\\projects.cpp\\studentdt.txt");
         int r;
        string cname,course,mobaile;
        while(getline(s,cname,'|')){
            getline(s,course,'|');
           s>>r;s.ignore();
        getline(s,mobaile);
        if(r==roll){
            nameh=cname;
            s.close();
            return true;
        }
    }
    s.close();
    return false;
        }
};
class libarary:public student{
    int id;
    string name,author,subject;
    int quantity;
    public:
    void addbook(){
        int num;
        cout<<"ENTER BOOK OF NUMBER :";
        cin>>num;
        for(int i=1;i<=num;i++){
            cout<<"BOOK="<<i<<endl;
        ofstream file("C:\\Users\\dc\\Desktop\\projects.cpp\\libarary1.txt",ios::app);
         
        cout<<"ENTER SUBJECT:";
        cin.ignore();
        getline(cin,subject);
        cout<<"ENTER BOOK ID: ";
        cin>>id;
        cout<<"ENTER BOOK NAME : ";
        cin.ignore();
        getline(cin,name);
        cout<<"ENTER AUTHOR NAME: ";
        cin.ignore();
        getline(cin,author);
        cout<<"ENTER QUANTITY: ";
        cin>>quantity;
        file<<subject<<"|"<<id<<"|"<<name<<"|"<<author<<"|"<<quantity<<endl;
       file.close();
       cout<<"BOOK ADDED SUCCESSFULLY "<<endl;
        }
    }
    void showallbook(){
        bool found=false;
        ifstream file("C:\\Users\\dc\\Desktop\\projects.cpp\\libarary1.txt");
        cout<<"=========ALL BOOKS=============\n";
   while(true){
    getline(file,subject,'|');
        if(!file)break;
        file>>id;file.ignore();
        getline(file,name,'|');
        getline(file,author,'|');
        file>>quantity;file.ignore();
           cout<<"SUBJECT="<<subject<<"\nID="<<id<<"\nBOOK NAME="<<name<<"\nAUTHOR NAME="<<author<<"\nBOOK QUANTITY="<<quantity<<endl;
           cout<<"================================="<<endl;
        }
        file.close();
    }
     void issubook(){ 
        int bookid,sroll;
        string stuname;
        bool found=false;
        cout<<"Enter Student Roll number:";
        cin>>sroll;
        if(!studentout(sroll,stuname)){
            cout<<"STUDENT NOT REGISTERED! ISSUE DENIED.\n";
            return;
        }
        cout<<"ENTER BOOK ID: ";
        cin>>bookid;
        ifstream file("C:\\Users\\dc\\Desktop\\projects.cpp\\libarary1.txt");
        ofstream file1("C:\\Users\\dc\\Desktop\\projects.cpp\\libararytemp.txt");
         while( getline(file,subject,'|')){
        file>>id;file.ignore();
        getline(file,name,'|');
        getline(file,author,'|');
        file>>quantity;file.ignore();
            if(bookid==id&&quantity>0){
                quantity--;
                found=true;
                ofstream his("C:\\Users\\dc\\Desktop\\projects.cpp\\libhistory.txt",ios::app);
                his<<stuname<<"|"<<id<<"|"<<name<<"|Issue|"<<currentDateTime();
                his.close();
                cout<<"Book Issue To :"<<stuname<<endl;
            }
             file1<<subject<<"|"<<id<<"|"<<name<<"|"<<author<<"|"<<quantity<<endl;
        }
        file.close();
        file1.close();
        remove("C:\\Users\\dc\\Desktop\\projects.cpp\\libarary1.txt");
        rename("C:\\Users\\dc\\Desktop\\projects.cpp\\libararytemp.txt","C:\\Users\\dc\\Desktop\\projects.cpp\\libarary1.txt");
        if(!found){
            cout<<"BOOK NOT AVILIBLE "<<endl;
        }
     }
     void returnbook(){
        int bookid,sroll;
        string stuname;
        bool found=false;
        cout<<"Enter Student Roll number:-";
        cin>>sroll;
       if(!studentout(sroll,stuname)){
            cout<<"STUDENT NOT REGISTERED! RETURN DENIED.\n";
            return;
        }
        cout<<"ENTER BOOK ID: ";
        cin>>bookid;
         ifstream file("C:\\Users\\dc\\Desktop\\projects.cpp\\libarary1.txt");
        ofstream file1("C:\\Users\\dc\\Desktop\\projects.cpp\\libararytemp.txt");
          while(getline(file,subject,'|')){
        file>>id;file.ignore();
        getline(file,name,'|');
        getline(file,author,'|');
        file>>quantity;file.ignore();
            if(id==bookid){
                quantity++;
                found=true;
                ofstream his("C:\\Users\\dc\\Desktop\\projects.cpp\\libhistory.txt",ios::app);
                his<<stuname<<"|"<<id<<"|"<<name<<"|Return|"<<currentDateTime();
                his.close();;
                cout<<"Book Returnd to:"<<stuname<<endl;
            }
            file1<<subject<<"|"<<id<<"|"<<name<<"|"<<author<<"|"<<quantity<<endl;
        }
        file.close();
        file1.close();
        remove("C:\\Users\\dc\\Desktop\\projects.cpp\\libarary1.txt");
        rename("C:\\Users\\dc\\Desktop\\projects.cpp\\libararytemp.txt","C:\\Users\\dc\\Desktop\\projects.cpp\\libarary1.txt");
        if(!found){
            cout<<"BOOK NOT FOUND: "<<endl;
        }

     }
     void showhistory(){
        ifstream his("C:\\Users\\dc\\Desktop\\projects.cpp\\libhistory.txt");
        string sname,action,datetime;
        cout<<"===================Libarary (Issue & Return) History=============\n";
        while(getline(his,sname,'|')){
            his>>id;his.ignore();
            getline(his,name,'|');
            getline(his,action);
            getline(his,datetime);
            cout<<"Student Name="<<sname<<endl;
            cout<<"Book id:="<<id<<endl;
            cout<<"Book Name:="<<name<<endl;
            cout<<"Book Action(Issue/return)="<<action<<endl;
            cout<<"DATE &  TIME:="<<datetime<<endl;
        }
        his.close();
     }
};
int main(){
    libarary l;
    int choice;
    do{
    cout<<"==================================================\n";
    cout<<"          LIBRARARY MANEGMENT SYSTEM \n";
    cout<<"==================================================\n";
    cout<<"1ADD STUDENT:\n";
    cout<<"2.ADDBOOK :\n";
    cout<<"3.ISSU BOOK:\n";
    cout<<"4.RETURN BOOK:\n";
    cout<<"5.SHOW ALL BOOK\n";
    cout<<"6.STUDENT DETAIL\n";
    cout<<"7.Show (Issue/Return) history\n";
    cout<<"8.EXIT\n";
    cout<<"ENTER CHOICE: ";
    cin>>choice;
    switch(choice){
        case 1: l.detail();break;
        case 2: l.addbook();break;
        case 3: l.issubook();break;
        case 4: l.returnbook();break;
        case 5: l.showallbook();break;
        case 6: l.showstdetail();break;
        case 7: l.showhistory();break;
        case 8: cout<<"EXIT THE PROGRAM ";break;
        default:
        cout<<"INVALID CHOICE"<<endl;
    }
    }while(choice!=8);
}
