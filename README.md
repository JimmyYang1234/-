//这是高中版的数学工具包
#include <stdio.h>
#include <math.h>
#include <stdlib.h>

// 函数声明
int is_prime(int n);
int gcd(int a, int b);
int lcm(int a, int b);
long long factorial(int n);
int is_narcissistic(int n);
void fibonacci_sequence(int n);
int reverse_number(int n);
int is_palindrome(int n);
void prime_checker();
void gcd_lcm_calculator();
void factorial_calculator();
void narcissistic_checker();
void fibonacci_generator();
void palindrome_checker();
void number_reverser();

// 判断质数
int is_prime(int n) {
    if (n < 2) return 0;
    if (n == 2) return 1;
    if (n % 2 == 0) return 0;

    for (int i = 3; i * i <= n; i += 2) {
        if (n % i == 0) return 0;
    }
    return 1;
}

// 计算最大公约数 - 欧几里得算法
int gcd(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}

// 计算最小公倍数
int lcm(int a, int b) {
    return a * b / gcd(a, b);
}

// 计算阶乘
long long factorial(int n) {
    if (n < 0) return -1;
    if (n == 0 || n == 1) return 1;

    long long result = 1;
    for (int i = 2; i <= n; i++) {
        result *= i;
    }
    return result;
}

// 判断水仙花数
int is_narcissistic(int n) {
    if (n < 0) return 0;

    int original = n, sum = 0, digits = 0;

    // 计算位数
    int temp = n;
    while (temp > 0) {
        digits++;
        temp /= 10;
    }

    // 计算各位数字的幂次和
    temp = n;
    while (temp > 0) {
        int digit = temp % 10;
        int power = 1;
        // 手动计算幂，避免浮点数精度问题
        for (int i = 0; i < digits; i++) {
            power *= digit;
        }
        sum += power;
        temp /= 10;
    }

    return sum == original;
}

// 斐波那契数列
void fibonacci_sequence(int n) {
    if (n <= 0) {
        printf("请输入正整数！\n");
        return;
    }

    printf("斐波那契数列前 %d 项: ", n);

    if (n >= 1) printf("0 ");
    if (n >= 2) printf("1 ");

    long long a = 0, b = 1;
    for (int i = 3; i <= n; i++) {
        long long next = a + b;
        printf("%lld ", next);
        a = b;
        b = next;
    }
    printf("\n");
}

// 数字反转
int reverse_number(int n) {
    int reversed = 0;
    int original = n;

    if (n < 0) n = -n;  // 处理负数

    while (n != 0) {
        reversed = reversed * 10 + n % 10;
        n /= 10;
    }

    return (original < 0) ? -reversed : reversed;
}

// 判断回文数
int is_palindrome(int n) {
    return n == reverse_number(n);
}

// 质数检查器功能
void prime_checker() {
    int num;
    printf("=== 质数检查器 ===\n");
    printf("请输入一个正整数: ");

    if (scanf("%d", &num) != 1 || num < 0) {
        printf("输入无效！请输入正整数。\n");
        while (getchar() != '\n'); // 清空输入缓冲区
        return;
    }

    if (is_prime(num)) {
        printf("%d 是质数\n", num);
    }
    else {
        printf("%d 不是质数\n", num);
    }

    // 显示该数以内的所有质数
    if (num > 1) {
        printf("\n%d 以内的质数有: ", num);
        int count = 0;
        for (int i = 2; i <= num; i++) {
            if (is_prime(i)) {
                printf("%d ", i);
                count++;
                if (count % 10 == 0) printf("\n");
            }
        }
        printf("\n共 %d 个质数\n", count);
    }
}

// 最大公约数和最小公倍数计算器
void gcd_lcm_calculator() {
    int num1, num2;
    printf("=== 最大公约数和最小公倍数计算器 ===\n");
    printf("请输入两个正整数: ");

    if (scanf("%d %d", &num1, &num2) != 2 || num1 <= 0 || num2 <= 0) {
        printf("输入无效！请输入两个正整数。\n");
        while (getchar() != '\n');
        return;
    }

    printf("最大公约数 (GCD): %d\n", gcd(num1, num2));
    printf("最小公倍数 (LCM): %d\n", lcm(num1, num2));
}

// 阶乘计算器
void factorial_calculator() {
    int num;
    printf("=== 阶乘计算器 ===\n");
    printf("请输入一个非负整数: ");

    if (scanf("%d", &num) != 1 || num < 0) {
        printf("输入无效！请输入非负整数。\n");
        while (getchar() != '\n');
        return;
    }

    long long result = factorial(num);
    if (result == -1) {
        printf("错误：阶乘不能为负数！\n");
    }
    else {
        printf("%d! = %lld\n", num, result);
    }
}

// 水仙花数检查器
void narcissistic_checker() {
    int num;
    printf("=== 水仙花数检查器 ===\n");
    printf("请输入一个正整数: ");

    if (scanf("%d", &num) != 1 || num < 0) {
        printf("输入无效！请输入正整数。\n");
        while (getchar() != '\n');
        return;
    }

    if (is_narcissistic(num)) {
        printf("%d 是水仙花数\n", num);
    }
    else {
        printf("%d 不是水仙花数\n", num);
    }

    // 显示一些已知的水仙花数
    printf("\n已知的水仙花数有: 153, 370, 371, 407\n");
}

// 斐波那契数列生成器
void fibonacci_generator() {
    int num;
    printf("=== 斐波那契数列生成器 ===\n");
    printf("请输入要显示的项数: ");

    if (scanf("%d", &num) != 1 || num <= 0) {
        printf("输入无效！请输入正整数。\n");
        while (getchar() != '\n');
        return;
    }

    fibonacci_sequence(num);
}

// 回文数检查器
void palindrome_checker() {
    int num;
    printf("=== 回文数检查器 ===\n");
    printf("请输入一个整数: ");

    if (scanf("%d", &num) != 1) {
        printf("输入无效！\n");
        while (getchar() != '\n');
        return;
    }

    if (is_palindrome(num)) {
        printf("%d 是回文数\n", num);
    }
    else {
        printf("%d 不是回文数\n", num);
    }
}

// 数字反转器
void number_reverser() {
    int num;
    printf("=== 数字反转器 ===\n");
    printf("请输入一个整数: ");

    if (scanf("%d", &num) != 1) {
        printf("输入无效！\n");
        while (getchar() != '\n');
        return;
    }

    printf("%d 反转后是 %d\n", num, reverse_number(num));
}

// 主菜单
int main() {
    int choice;

    printf("=== 数学工具包 (高中版) ===\n");
    printf("作者: 杨博凯\n\n");

    do {
        printf("\n请选择功能:\n");
        printf("1. 质数检查器\n");
        printf("2. 最大公约数和最小公倍数\n");
        printf("3. 阶乘计算器\n");
        printf("4. 水仙花数检查器\n");
        printf("5. 斐波那契数列生成器\n");
        printf("6. 回文数检查器\n");
        printf("7. 数字反转器\n");
        printf("0. 退出程序\n");
        printf("请输入选择 (0-7): ");

        if (scanf("%d", &choice) != 1) {
            printf("输入无效！请重新输入。\n");
            while (getchar() != '\n');
            continue;
        }

        switch (choice) {
        case 1: prime_checker(); break;
        case 2: gcd_lcm_calculator(); break;
        case 3: factorial_calculator(); break;
        case 4: narcissistic_checker(); break;
        case 5: fibonacci_generator(); break;
        case 6: palindrome_checker(); break;
        case 7: number_reverser(); break;
        case 0: printf("感谢使用数学工具包，再见！\n"); break;
        default: printf("无效选择！请输入0-7之间的数字。\n");
        }

        if (choice != 0) {
            printf("\n按Enter键继续...");
            while (getchar() != '\n'); // 清空缓冲区
            getchar(); // 等待用户按Enter
        }

    } while (choice != 0);

    return 0;
}
