#include <iostream>
#include <cstdlib>
#include <cmath>
#include <chrono>
#include <future>
#include <thread>

// Функция для обновления позиции грибника
void update_position(double& x, double& y, double& total_distance, double angle, double speed, double time) {
    x += speed * cos(angle) * time; // Обновляем координату x
    y += speed * sin(angle) * time; // Обновляем координату y
    total_distance += speed * time;  // Обновляем общее расстояние
}

// Функция для сбора грибов
int collect_mushroom(int mushrooms_collected) {
    return mushrooms_collected + 1; // Увеличиваем и возвращаем счетчик собранных грибов
}

// Функция для охоты на грибы
void mushroom_hunting(double& x, double& y, double& total_distance, int& mushrooms_collected, int T) {
    std::srand(static_cast<unsigned int>(std::time(nullptr))); // Инициализация генератора случайных чисел

    for (int i = 0; i < T; ++i) {
        // Генерируем случайный угол в радианах
        double angle = static_cast<double>(std::rand()) / RAND_MAX * 2 * M_PI; // Угол от 0 до 2π
        double speed = 1.0; // Скорость грибника
        double time = 1.0;  // Время движения

        // Создаем асинхронное ожидание обновления позиции
        std::future<void> update_future = std::async(std::launch::async, update_position, std::ref(x), std::ref(y),
                                                      std::ref(total_distance), angle, speed, time);

        // Собираем грибы (можно делать это после обновления позиции для ускорения)
        mushrooms_collected = collect_mushroom(mushrooms_collected);

        // Ждем завершения обновления позиции
        update_future.get();
    }
}

int main() {
    double x = 0.0;                       // Начальная координата x
    double y = 0.0;                       // Начальная координата y
    double total_distance = 0.0;          // Общее расстояние
    int mushrooms_collected = 0;          // Количество собранных грибов
    int T = 10;                           // Общее количество итераций охоты

    // Запуск охоты на грибы
    mushroom_hunting(x, y, total_distance, mushrooms_collected, T);

    // Вывод результатов
    std::cout << "Общая дистанция: " << total_distance << std::endl;
    std::cout << "Собрано грибов: " << mushrooms_collected << std::endl;
    std::cout << "Конечные координаты: (" << x << ", " << y << ")" << std::endl;

    return 0;
}