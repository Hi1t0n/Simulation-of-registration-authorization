#include <iostream>
#include <random>
#include <string>
using namespace std;
using std::string;
std::random_device rd;

// Функции

string generate_password(size_t len = 7) // генерация кода доступа
{
    string res;
    while (len--)
        res += '0' + rd() % 10;
    return res;
}



string change_generate_password(string pass) // генерация нового кода доступа
{
    for (auto& ch : pass)
        ch = '0' + ((ch - '0') + 1) % 10;
    return pass;
}



int main()
{
    
    setlocale(LC_ALL, "Russian");

    // Переменные
    
    string login,last_login, name, password, new_password, last_password,code,last_code,new_code;
    int menu, count_, change_password;
    count_ = 0;
    change_password = 0;
    int number = 0;
    int const SIZE = 1000;
    int i = 0;
    int value = 0;
    int index;
    int check_login = 0;
    
    // массивы
    
    string *all_login = new string[SIZE]; // логины
    string* all_password = new string[SIZE]; // пароли
    string* all_code = new string[SIZE]; // одноразовые пароли(Коды)
    string* all_name = new string[SIZE]; // имена пользователей

    // Меню
    while (true)
    {
        

    cout <<"Выберите нужное действие:\n"
        <<"1. Регистрация\n"
        <<"2. Авторизация\n"
        <<"3. Смена пароля\n"
        <<"4. Выход\n";
    cin >> menu;
    
        switch (menu)
        {
            // Регистрация
        case 1:
            cout << "Добро пожаловать в меню регистрации!!!" << endl;
            cout << "Введите логин: ";
            cin >> login;
            
            for (; check_login < SIZE; check_login++)
            {
                if (login != all_login[check_login])
                {
                    last_login = login;
                    password = generate_password();
                    last_password = password;

                    cout << "Введите ваше имя: ";
                    cin >> name;
                    code = generate_password(); // генерируется одноразовый код доступа
                    last_code = code;

                    cout << "Ваш логин: " << login
                        << "\nВаш пароль: " << password
                        << "\nВаш одноразовый код доступа: " << code << endl << endl;

                    change_password++;

                    for (; value < 1; i++)
                    {
                        all_login[i] = login;
                        all_password[i] = password;
                        all_code[i] = code;
                        all_name[i] = name;
                        value++;
                    }
                    value = 0;

                    
                    check_login = 0;
                    break;
                }
                else
                {
                    check_login = 0;
                    cout << "Этот логин уже занят!!!" << endl << endl;
                    break;
                }
            }
            break;


            // Авторизация
        case 2:
            if(change_password>0)
            {
                while (true)
                {

                    cout << "Введите ваш логин: ";
                    cin >> login;
                    cout << "Введите ваш пароль: ";
                    cin >> password;
                    cout << "Введите ваш код доступа: ";
                    cin >> code;
                    int b = 0;
                    for (; b < SIZE; b++)
                    {
                        if (login == all_login[b])
                        {
                            index = b;
                            continue;
                        }
                    }


                    if (login == all_login[index] && password == all_password[index] && code == all_code[index])
                    {
                        last_code = code;
                        cout << "Здравствуйте! " << all_name[index] << endl;
                        code = change_generate_password(last_code);
                        last_code = code;
                        all_code[index] = code;
                        cout << "Ваш новый код доступа: " << code << endl<<endl;
                        break;

                    }
                    else
                    {
                        cout << "Логин пароль или Код доступа не верны!\n" << endl;
                        count_++;

                        if (count_ > 3)
                        {
                            cout << "Смените пароль!\n";
                            count_ = 0;
                            
                        }

                    }
                    break;
                }
                break;

            }
            // Смена пароля
        case 3:
            if (change_password > 0)
            {
                cout << "Введите ваш логин: ";
                cin >> login;
                cout << "Введите ваш код доступа: ";
                cin >> code;

                int v = 0;
                for (; v < SIZE; v++)
                {
                    if (login == all_login[v])
                    {
                        index = v;
                        continue;
                    }
                }

                if (login == all_login[index] && code == all_code[index])
                {
                    new_password = generate_password();
                    password = new_password;
                    last_password = password;
                    all_password[index] = password;
                    last_code = code;
                    new_code = change_generate_password(last_code);
                    code = new_code;
                    last_code = code;
                    all_code[index] = code;
                    cout << "Ваш логин: " << all_login[index]
                        << "\nВаш новый пароль: " << all_password[index]
                        << "\nВаш новый код доступа: " << all_code[index] << endl << endl;
                }

            }
            else
            {
                cout << "Вы еще не зарегестрированны!!!" << endl << endl;
            }
            break;

        // Выход
        case 4:
            // очищаем память
            delete [] all_login;
            delete [] all_password;
            delete [] all_code;
            delete[] all_name;
            return 0;

        default:
            cout << "Такого пункта в меню нет попробуйте еще раз";
            break;
        }
    }
}
