#include <iostream>
#include <stdio.h>
#include <stdlib.h>
#include <string>

using namespace std;

char Encoder[44], Offset[44], msg[1000], encd[1000];
int i, j, c,k;

void Reference() {                                       //////////// Main list
    int c = 65;
    for (int i = 0; i < 44; i++) {
        Encoder[i] = c;
        c++;
        if (c == 91) {
            c = 48;
        }
        else if (c == 58) {
            c = 40;
        }
    }
}

void ChngList(int c) {                                  /////////// Offset list
    int i, j;

    for (i = 44 - c, j = 0; i < 44; i++, j++) {
        Offset[j] = Encoder[i];
    }
    for (i = 0; j < 44; i++, j++) {
        Offset[j] = Encoder[i];
    }
    /*for (i = 0; i < 44; i++) {
        cout << Offset[i] << ' ';
    }*/
    cout << endl;
}

char CaseSense(char a) {                                /////////// Avoids Case sensitivity issue
    if (islower(a)) {
        a = toupper(a);
    }
    return a;
}

void ReadMsg() {                                       //////////// Takes msg as input
    cout<<"Enter message : ";
    cin.getline(msg, 1000);
    for (int i = 0; i < strlen(msg); i++) {
        msg[i] = CaseSense(msg[i]);
    }
    //cout << msg << endl;
}

void Encod() {                                        ////////////// ENCODING ///////////////
    
    system("CLS");
    char ref;
    cout << "Enter Rerefence value from the list :" << endl;
    Reference();
    for (i = 0; i < 44; i++)
        cout << Encoder[i] << ' ';
    cout << '\n';
    cin >> ref;
    ref = CaseSense(ref);
    //cout << ref << endl;
    cin.ignore();

    ReadMsg();

    for (i = 0; i < 44; i++) {
        if (ref == Encoder[i]) {
            c = i;
            break;
        }
    }
    encd[0] = ref;

    ChngList(c);

    for (i = 0, k = 1; i < strlen(msg); i++) {
        for (j = 0; j < 44; j++) {
            if (msg[i] == ' ')
                encd[k] = ' ';
            if (Encoder[j] == msg[i]) {
                encd[k] = Offset[j];
            }
        }
        k++;
    }
    encd[k] = '\0';

    cout << encd << endl << "Press enter to continue...";;
    cin.ignore();
    system("CLS");
}

 void Decoder() {                                           ///////////////// DECODING /////////////
     
     system("CLS");
     char decod[1000];
     Reference();
     ReadMsg();
     
     for (i = 0; i < 44; i++) {
         if (msg[0] == Encoder[i]) {
             c = i;
             break;
         }
     }
     ChngList(c);
     
     for (i = 1,k=0; i < strlen(msg); i++) {
         for (j = 0; j < 44; j++) {
             if (msg[i] == ' ')
                 decod[k] = ' ';
             if (Offset[j] == msg[i]) {
                 decod[k] = Encoder[j];
             }
         }
         k++;
     }
     decod[k] = '\0';
     cout << decod << endl <<"Press enter to continue...";
     cin.ignore();
     system("CLS");
 }

int main() {

    int option;
    while (1) {
        cout << " Key in your option " << endl << " 1. Encode \n 2. Decode \n 3. Exit \n \n ";
        cin >> option;
        if (option == 1) {
            Encod();
        }
        else if (option == 2) {
            cin.ignore();
            Decoder();
        }
        else if (option == 3) {
            exit(0);
        }
        else{
            cout << "Invalid Entry \n";
        }
    }
   
    return 0;

}
