#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct Tab {
    int pageID;
    char url[100];
    struct Tab *prev, *next;
} Tab;

Tab *head = NULL, *current = NULL;
int pageCounter = 1;

// Function to create a new tab
Tab* createTab(char url[]) {
    Tab *newTab = (Tab*)malloc(sizeof(Tab));
    newTab->pageID = pageCounter++;
    strcpy(newTab->url, url);
    newTab->prev = newTab->next = NULL;
    return newTab;
}

// Visit a new page
void visitNewPage() {
    char url[100];
    printf("Enter URL: ");
    scanf("%s", url);

    Tab *newTab = createTab(url);

    if (head == NULL) {
        head = newTab;
        current = newTab;
    } else {
        current->next = newTab;
        newTab->prev = current;
        current = newTab;
    }

    printf("Visited: %s (PageID: %d)\n", current->url, current->pageID);
}

// Move forward
void goForward() {
    if (current != NULL && current->next != NULL) {
        current = current->next;
        printf("Moved Forward → %s (PageID: %d)\n", current->url, current->pageID);
    } else {
        printf("No next tab available!\n");
    }
}

// Move backward
void goBack() {
    if (current != NULL && current->prev != NULL) {
        current = current->prev;
        printf("Moved Back ← %s (PageID: %d)\n", current->url, current->pageID);
    } else {
        printf("No previous tab available!\n");
    }
}

// Show current tab
void showCurrentTab() {
    if (current != NULL) {
        printf("Current Tab → %s (PageID: %d)\n", current->url, current->pageID);
    } else {
        printf("No tab open!\n");
    }
}

// Close current tab
void closeCurrentTab() {
    if (current == NULL) {
        printf("No tab to close!\n");
        return;
    }

    printf("Closing tab: %s (PageID: %d)\n", current->url, current->pageID);

    if (current->prev != NULL)
        current->prev->next = current->next;
    if (current->next != NULL)
        current->next->prev = current->prev;

    Tab *temp = NULL;
    if (current->next != NULL)
        temp = current->next;
    else
        temp = current->prev;

    if (current == head)
        head = current->next;

    free(current);
    current = temp;
}

// Show all history
void showHistory() {
    Tab *temp = head;
    if (temp == NULL) {
        printf("No history available!\n");
        return;
    }

    printf("Browser History:\n");
    while (temp != NULL) {
        printf("PageID: %d, URL: %s\n", temp->pageID, temp->url);
        temp = temp->next;
    }
}

int main() {
    int choice;
    while (1) {
        printf("\n===== Browser Menu =====\n");
        printf("1. Visit a New Page\n");
        printf("2. Go Back\n");
        printf("3. Go Forward\n");
        printf("4. Show Current Tab\n");
        printf("5. Close Current Tab\n");
        printf("6. Show History\n");
        printf("7. Exit\n");
        printf("Enter choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: visitNewPage(); break;
            case 2: goBack(); break;
            case 3: goForward(); break;
            case 4: showCurrentTab(); break;
            case 5: closeCurrentTab(); break;
            case 6: showHistory(); break;
            case 7: exit(0);
            default: printf("Invalid choice!\n");
        }
    }
    return 0;
}
