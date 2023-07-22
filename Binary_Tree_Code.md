#include <iostream>
#define SPACE 10

using namespace std;

// Deklarasi kelas Node
class Node {
public:
int data;
Node* left;
Node* right;

// Konstruktor default
Node() {
    data = 0;
    left = NULL;
    right = NULL;
}

// Konstruktor dengan parameter
Node(int v) {
    data = v;
    left = NULL;
    right = NULL;
}
};

// Deklarasi kelas BinaryTree
class BinaryTree {
public:
Node* root;

// Konstruktor default
BinaryTree() {
    root = NULL;
}

// Fungsi untuk memeriksa apakah Binary Tree kosong
bool isTreeEmpty() {
    return root == NULL;
}

// Fungsi untuk menyisipkan (insert) data ke dalam Binary Tree
void insert(int val) {
    Node* new_node = new Node(val);

    if (root == NULL) {
        root = new_node;
        cout << "Data Berhasil di Masukkan" << endl;
    } else {
        insertNode(root, new_node);
    }
}

// Fungsi rekursif untuk menyisipkan (insert) data ke dalam Binary Tree
void insertNode(Node* current, Node* new_node) {
    if (new_node->data < current->data) {
        if (current->left == NULL) {
            current->left = new_node;
            cout << "Data Berhasil di Masukkan" << endl;
        } else {
            insertNode(current->left, new_node);
        }
    } else if (new_node->data > current->data) {
        if (current->right == NULL) {
            current->right = new_node;
            cout << "Data Berhasil di Masukkan" << endl;
        } else {
            insertNode(current->right, new_node);
        }
    } else {
        cout << "Data Sudah Ada!" << endl;
    }
}

// Fungsi untuk melakukan penelusuran InOrder pada Binary Tree
void traverseInOrder(Node* current) {
    if (current == NULL) {
        return;
    }

    traverseInOrder(current->left);
    cout << current->data << " ";
    traverseInOrder(current->right);
}

// Fungsi untuk melakukan penelusuran PreOrder pada Binary Tree
void traversePreOrder(Node* current) {
    if (current == NULL) {
        return;
    }

    cout << current->data << " ";
    traversePreOrder(current->left);
    traversePreOrder(current->right);
}

// Fungsi untuk melakukan penelusuran PostOrder pada Binary Tree
void traversePostOrder(Node* current) {
    if (current == NULL) {
        return;
    }

    traversePostOrder(current->left);
    traversePostOrder(current->right);
    cout << current->data << " ";
}

// Fungsi untuk mencari data pada Binary Tree
Node* search(int val) {
    return searchNode(root, val);
}

// Fungsi rekursif untuk mencari data pada Binary Tree
Node* searchNode(Node* current, int val) {
    if (current == NULL || current->data == val) {
        return current;
    } else if (val < current->data) {
        return searchNode(current->left, val);
    } else {
        return searchNode(current->right, val);
    }
}

// Fungsi untuk menghapus simpul (node) pada Binary Tree
Node* deleteNode(Node* current, int value) {
    if (current == NULL) {
        return current;
    }

    if (value < current->data) {
        current->left = deleteNode(current->left, value);
    } else if (value > current->data) {
        current->right = deleteNode(current->right, value);
    } else {
        if (current->left == NULL) {
            Node* temp = current->right;
            delete current;
            return temp;
        } else if (current->right == NULL) {
            Node* temp = current->left;
            delete current;
            return temp;
        }

        Node* temp = minValueNode(current->right);
        current->data = temp->data;
        current->right = deleteNode(current->right, temp->data);
    }

    return current;
}

// Fungsi untuk mencari simpul dengan nilai terkecil pada Binary Tree
Node* minValueNode(Node* current) {
    Node* temp = current;
    while (temp && temp->left != NULL) {
        temp = temp->left;
    }
    return temp;
}

// Fungsi untuk menghapus semua simpul pada Binary Tree
void clear() {
    clearTree(root);
    root = NULL;
    cout << "Binary Tree Cleared." << endl;
}

// Fungsi rekursif untuk menghapus semua simpul pada Binary Tree
void clearTree(Node* current) {
    if (current == NULL) {
        return;
    }

    clearTree(current->left);
    clearTree(current->right);
    delete current;
}

// Fungsi untuk menampilkan Binary Tree
void display() {
    displayTree(root, 0);
}

// Fungsi rekursif untuk menampilkan Binary Tree dengan format indentasi
void displayTree(Node* current, int space) {
    if (current == NULL) {
        return;
    }

    space += SPACE;
    displayTree(current->right, space);
    cout << endl;

    for (int i = SPACE; i < space; i++) {
        cout << " ";
    }
    cout << current->data << "\n";

    displayTree(current->left, space);
}
};

int main() {
BinaryTree tree;
int option, val;


do {
    system("cls");
    cout << "BINARY TREE DISPLAY: " << endl;
    tree.display();
    cout << endl;

    cout << "\n============ MENU : BINARY TREE ============ " << endl;
    cout << "| 1. Insert Node                           |" << endl;
    cout << "| 2. Transverse                            |" << endl;
    cout << "| 3. Search Node                           |" << endl;
    cout << "| 4. Delete Node                           |" << endl;
    cout << "| 5. Clear Node                            |" << endl;
    cout << "| 6. Exit Program                          |" << endl;
    cout << "============================================ " << endl;
    cout << "Pilih menu: ";
    cin >> option;
    cout << endl;

    switch (option) {
        case 1:
            cout << "SISIPKAN NODE KE BINARY TREE" << endl;
            cout << "Masukkan data TREE NODE yang ingin disisipkan ke dalam Binary Tree: ";
            cin >> val;
            tree.insert(val);
            cout << endl;
            break;
        case 2:
            cout << "PENELUSURAN BINARY TREE" << endl;
            cout << "InOrder: ";
            tree.traverseInOrder(tree.root);
            cout << endl;

            cout << "PreOrder: ";
            tree.traversePreOrder(tree.root);
            cout << endl;

            cout << "PostOrder: ";
            tree.traversePostOrder(tree.root);
            cout << endl;
            break;
        case 3:
            cout << "PENCARIAN NODE BINARY TREE" << endl;
            cout << "Masukkan data TREE NODE yang ingin dicari di dalam Binary Tree: ";
            cin >> val;
            if (tree.search(val) != NULL) {
                cout << "Data ditemukan dalam Binary Tree." << endl;
            } else {
                cout << "Data tidak ditemukan dalam Binary Tree." << endl;
            }
            cout << endl;
            break;
        case 4:
            cout << "HAPUS NODE" << endl;
            cout << "Masukkan nilai yang ingin dihapus: ";
            cin >> val;
            tree.root = tree.deleteNode(tree.root, val);
            cout << "Node dengan nilai " << val << " telah dihapus." << endl << endl;
            break;
        case 5:
            cout << "BERSIHKAN BINARY TREE" << endl;
            tree.clear();
            cout << endl;
            break;
         case 6:
            break;
        default:
            cout << "Pilihan tidak valid. Mohon masukkan nomor menu yang valid." << endl;
    }
    system("pause");
} while (option != 6);

return 0;
}
