#include <iostream>
#include <cstdlib>
#include <cmath>
#include <chrono>
#include <future>
#include <thread>
#include <vector>

const double M_PI = 3.14;
using namespace std;

// Структура для хранения состояния грибника
struct MushroomPicker {
    double x = 0.0; // Координата x
    double y = 0.0; // Координата y
    double total_distance = 0.0; // Общая длина пути
    int mushrooms_collected = 0; // Количество собранных грибов
};

// Функция для обновления позиции грибника
void update_position(MushroomPicker& picker, double angle, double speed, double time) {
    picker.x += speed * cos(angle) * time; // Обновляем координату x
    picker.y += speed * sin(angle) * time; // Обновляем координату y
    picker.total_distance += speed * time;  // Обновляем общее расстояние
}

// Функция для сбора грибов и общения с другом
void mushroom_hunting(MushroomPicker& picker, MushroomPicker& friend_picker, int T, double U) {
    srand(static_cast<unsigned int>(time(nullptr))); // Инициализация генератора случайных чисел
    double angle, speed, time, distance_to_start, friend_distance_to_start, my_distance;
    for (int i = 0; i < T; ++i) {
        // Генерируем случайный угол в радианах
         angle = (static_cast<double>(rand()) / RAND_MAX) * 2 * M_PI; // угол от 0 до 2π

        // Генерируем случайную скорость и время движения
         speed = (static_cast<double>(rand()) / RAND_MAX) * U; // скорость от 0 до U
         time = (static_cast<double>(rand()) / RAND_MAX) * 7; // время от 0 до 7

        // Обновляем позицию грибника
        update_position(picker, angle, speed, time);

        // Собираем грибы
        picker.mushrooms_collected++;

        // Вычисляем расстояния
         distance_to_start = sqrt(picker.x * picker.x + picker.y * picker.y);
         

         // Другой грибник тоже обновляет свои координаты
         update_position(friend_picker, angle, speed, time);
         friend_picker.mushrooms_collected++;

         // Вычисляем расстояния для друга
         friend_distance_to_start = sqrt(friend_picker.x * friend_picker.x + friend_picker.y * friend_picker.y);
         my_distance = sqrt((friend_picker.x - picker.x) * (friend_picker.x - picker.x) +
             (friend_picker.y - picker.y) * (friend_picker.y - picker.y));


    }
        // Вывод результатов
        cout << "Грибник: (" << picker.x << ", " << picker.y << "), Дистанция до старта: " << distance_to_start
            << ", Дистанция до друга: " << my_distance << ", Грибов: " << 0 << endl;

        // Вывод результатов друга
        cout << "Друг: (" << friend_picker.x << ", " << friend_picker.y << "), Дистанция до старта: "
            << friend_distance_to_start << ", Дистанция до грибника: " << my_distance
            << ", Грибов: " << 0 << endl;
    
}

int main() {
    setlocale(LC_ALL, "RU");

    MushroomPicker picker1; // Первый грибник
    MushroomPicker picker2; // Второй грибник
    int T = 0; // Общее количество итераций охоты
    double U = 10.0; // Максимальная скорость
    cout << "Введите количество собранныз грибов" << endl;
    cin >> T;

    // Запуск охоты на грибы
    mushroom_hunting(picker1, picker2, T, U);

    // Вывод финальных результатов
    cout << "Финальный результат грибника 1: Общая дистанция: " << picker1.total_distance
        << ", Собрано грибов: " << picker1.mushrooms_collected << ", Конечные координаты: ("
        << picker1.x << ", " << picker1.y << ")" << endl;

    cout << "Финальный результат грибника 2: Общая дистанция: " << picker2.total_distance
        << ", Собрано грибов: " << picker2.mushrooms_collected << ", Конечные координаты: ("
        << picker2.x << ", " << picker2.y << ")" << endl;

    return 0;
}
