# 智能通讯管理系统
大一新生学完C语言的第一个作品，闲来无事，想练习一下，网上找的教程
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#define MAX_CONTACTS 100
#define PHONE_LEN 12
#define NAME_LEN 20

// 结构体定义
typedef struct 
{
    char name[NAME_LEN];
    char phone[PHONE_LEN];
    int id;
    time_t create_time;
} Contact;

// 全局变量
Contact contacts[MAX_CONTACTS];
int contact_count = 0;

// 函数声明
void add_contact();
void delete_contact();
void search_contact();
void display_all_contacts();
void sort_contacts();
void generate_random_contacts();
int binary_search_contact(const char* name);
void save_to_file();
void load_from_file();

// 主函数
int main() {
    int choice;

    printf("=== 智能通讯录管理系统 ===\n");
    load_from_file();

    do {
        printf("\n1. 添加联系人\n");
        printf("2. 删除联系人\n");
        printf("3. 查找联系人\n");
        printf("4. 显示所有联系人\n");
        printf("5. 排序联系人\n");
        printf("6. 生成随机测试数据\n");
        printf("7. 保存到文件\n");
        printf("0. 退出\n");
        printf("请选择操作: ");
        scanf("%d", &choice);

        switch (choice) {
        case 1: add_contact(); break;
        case 2: delete_contact(); break;
        case 3: search_contact(); break;
        case 4: display_all_contacts(); break;
        case 5: sort_contacts(); break;
        case 6: generate_random_contacts(); break;
        case 7: save_to_file(); break;
        case 0: printf("再见!\n"); break;
        default: printf("无效选择!\n");
        }
    } while (choice != 0);

    return 0;
}

// 添加联系人
void add_contact() {
    if (contact_count >= MAX_CONTACTS) {
        printf("通讯录已满!\n");
        return;
    }

    Contact* new_contact = &contacts[contact_count];

    printf("请输入姓名: ");
    scanf("%s", new_contact->name);
    printf("请输入电话号码: ");
    scanf("%s", new_contact->phone);

    new_contact->id = contact_count + 1;
    new_contact->create_time = time(NULL);

    contact_count++;
    printf("联系人添加成功! ID: %d\n", new_contact->id);
}

// 二分查找联系人
int binary_search_contact(const char* name) {
    int left = 0, right = contact_count - 1;

    while (left <= right) {
        int mid = left + (right - left) / 2;
        int cmp = strcmp(contacts[mid].name, name);

        if (cmp == 0) return mid;
        else if (cmp < 0) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}

// 查找联系人
void search_contact() {
    if (contact_count == 0) {
        printf("通讯录为空!\n");
        return;
    }

    char name[NAME_LEN];
    printf("请输入要查找的姓名: ");
    scanf("%s", name);

    // 先排序确保二分查找有效
    sort_contacts();
    int index = binary_search_contact(name);

    if (index != -1) {
        printf("找到联系人:\n");
        printf("ID: %d, 姓名: %s, 电话: %s\n",
            contacts[index].id, contacts[index].name, contacts[index].phone);
    }
    else {
        printf("未找到联系人: %s\n", name);
    }
}

// 冒泡排序联系人
void sort_contacts() {
    for (int i = 0; i < contact_count - 1; i++) {
        for (int j = 0; j < contact_count - i - 1; j++) {
            if (strcmp(contacts[j].name, contacts[j + 1].name) > 0) {
                Contact temp = contacts[j];
                contacts[j] = contacts[j + 1];
                contacts[j + 1] = temp;
            }
        }
    }
    printf("联系人已按姓名排序!\n");
}

// 生成随机测试数据
void generate_random_contacts() {
    char* names[] = { "张三", "李四", "王五", "赵六", "钱七", "孙八", "周九", "吴十" };
    char* phones[] = { "13800138000", "13900139000", "13600136000", "13700137000" };

    srand(time(NULL));
    int count = rand() % 5 + 3; // 生成3-7个随机联系人

    for (int i = 0; i < count && contact_count < MAX_CONTACTS; i++) {
        Contact* new_contact = &contacts[contact_count];

        strcpy(new_contact->name, names[rand() % 8]);
        strcpy(new_contact->phone, phones[rand() % 4]);
        // 修改电话号码后几位
        new_contact->phone[9] = '0' + (rand() % 10);
        new_contact->phone[10] = '0' + (rand() % 10);

        new_contact->id = contact_count + 1;
        new_contact->create_time = time(NULL);
        contact_count++;
    }
    printf("成功生成 %d 个随机联系人!\n", count);
}

// 其他函数实现...
void delete_contact() {
    if (contact_count == 0) {
        printf("通讯录为空!\n");
        return;
    }

    int id;
    printf("请输入要删除的联系人ID: ");
    scanf("%d", &id);

    for (int i = 0; i < contact_count; i++) {
        if (contacts[i].id == id) {
            for (int j = i; j < contact_count - 1; j++) {
                contacts[j] = contacts[j + 1];
            }
            contact_count--;
            printf("联系人删除成功!\n");
            return;
        }
    }
    printf("未找到ID为 %d 的联系人!\n", id);
}

void display_all_contacts() {
    if (contact_count == 0) {
        printf("通讯录为空!\n");
        return;
    }

    printf("\n=== 所有联系人 ===\n");
    for (int i = 0; i < contact_count; i++) {
        printf("ID: %d, 姓名: %-10s, 电话: %s\n",
            contacts[i].id, contacts[i].name, contacts[i].phone);
    }
}

void save_to_file() {
    FILE* file = fopen("contacts.dat", "wb");
    if (file == NULL) {
        printf("文件保存失败!\n");
        return;
    }

    fwrite(&contact_count, sizeof(int), 1, file);
    fwrite(contacts, sizeof(Contact), contact_count, file);
    fclose(file);
    printf("数据已保存到文件!\n");
}

void load_from_file() {
    FILE* file = fopen("contacts.dat", "rb");
    if (file == NULL) {
        printf("未找到保存的数据文件\n");
        return;
    }

    fread(&contact_count, sizeof(int), 1, file);
    fread(contacts, sizeof(Contact), contact_count, file);
    fclose(file);
    printf("数据已从文件加载! 共 %d 个联系人\n", contact_count);
}
