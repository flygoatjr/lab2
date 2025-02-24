#include <stdio.h>
#include <stdlib.h>
#include <locale.h>

int convertTo(int number, int baseFrom) {
    int dValue = 0;
    int power = 1;

    while (number > 0) {
        int digit = number % 10;
        dValue += digit * power;
        power *= baseFrom;
        number /= 10;
    }

    return dValue;
}

void convertFrom(int dValue, int baseTo) {
    char result[32];
    int index = 0;

    if (dValue == 0) {
        printf("0");
        return;
    }

    while (dValue > 0) {
        result[index++] = (dValue % baseTo) + '0';
        dValue /= baseTo;
    }

    for (int i = index - 1; i >= 0; i--) {
        printf("%c", result[i]);
    }
}

int main() {
    int number, baseFrom, baseTo;

    setlocale(LC_ALL, "RU");

    while (1) {
        printf("Введите основание исходной системы (от 2 до 9): ");
        scanf_s("%d", &baseFrom);

        printf("Введите основание результирующей системы (от 2 до 9): ");
        scanf_s("%d", &baseTo);

        if (baseFrom < 2 || baseFrom > 9 || baseTo < 2 || baseTo > 9) {
            printf("Основание должно быть от 2 до 9.\n");
            continue;
        }

        printf("Введите натуральное число в системе с основанием %d: ", baseFrom);
        scanf_s("%d", &number);

        int temp = number;
        while (temp > 0) {
            if (temp % 10 >= baseFrom) {
                printf("Число должно содержать цифры, допустимые для системы с основанием %d.\n", baseFrom);
                break;
            }
            temp /= 10;
        }

        if (temp == 0) {
            int decimalValue = convertTo(number, baseFrom);
            printf("Число %d в системе с основанием %d: ", number, baseTo);
            convertFrom(decimalValue, baseTo);
            printf("\n");
        }

        char choice;
        printf("Хотите продолжить? (y/n): ");
        scanf_s(" %c", &choice);
        if (choice != 'y' && choice != 'Y') {
            break;
        }
    }

    return 0;
}
