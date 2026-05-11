#include <iostream>
#include <vector>
#include <fstream>
#include <algorithm>
using namespace std;

class Product
{
private:
    int productID;
    string productName;
    string category;
    float price;

public:
    // Constructor
    Product(int id, string name, string cat, float pr)
    {
        productID = id;
        productName = name;
        category = cat;
        price = pr;
    }

    // Display product details
    void display()
    {
        cout << "\nProduct ID   : " << productID;
        cout << "\nProduct Name : " << productName;
        cout << "\nCategory     : " << category;
        cout << "\nPrice        : Rs." << price << endl;
    }

    // Getter functions
    int getID()
    {
        return productID;
    }

    string getCategory()
    {
        return category;
    }

    float getPrice()
    {
        return price;
    }

    // Save product to file
    void saveToFile(ofstream &file)
    {
        file << productID << " "
             << productName << " "
             << category << " "
             << price << endl;
    }
};

// Function to display all products
void displayAll(vector<Product> &products)
{
    if(products.empty())
    {
        cout << "\nNo products available!\n";
        return;
    }

    for(int i = 0; i < products.size(); i++)
    {
        products[i].display();
    }
}

// Search by category
void searchCategory(vector<Product> &products, string cat)
{
    bool found = false;

    for(int i = 0; i < products.size(); i++)
    {
        if(products[i].getCategory() == cat)
        {
            products[i].display();
            found = true;
        }
    }

    if(!found)
    {
        cout << "\nNo products found in this category!\n";
    }
}

// Search by product ID
void searchID(vector<Product> &products, int id)
{
    bool found = false;

    for(int i = 0; i < products.size(); i++)
    {
        if(products[i].getID() == id)
        {
            products[i].display();
            found = true;
        }
    }

    if(!found)
    {
        cout << "\nProduct not found!\n";
    }
}

// Sort products by price
bool comparePrice(Product a, Product b)
{
    return a.getPrice() < b.getPrice();
}

void sortProducts(vector<Product> &products)
{
    sort(products.begin(), products.end(), comparePrice);

    cout << "\nProducts Sorted by Price Successfully!\n";
}

// Save all products to file
void saveProducts(vector<Product> &products)
{
    ofstream file("products.txt");

    for(int i = 0; i < products.size(); i++)
    {
        products[i].saveToFile(file);
    }

    file.close();

    cout << "\nProducts saved to file successfully!\n";
}

int main()
{
    vector<Product> products;

    int choice;

    do
    {
        cout << "\n========== PRODUCT CATEGORIZATION SYSTEM ==========";
        cout << "\n1. Add Product";
        cout << "\n2. Display All Products";
        cout << "\n3. Search by Category";
        cout << "\n4. Search by Product ID";
        cout << "\n5. Sort Products by Price";
        cout << "\n6. Save Products to File";
        cout << "\n7. Exit";
        cout << "\nEnter your choice: ";
        cin >> choice;

        switch(choice)
        {
        case 1:
        {
            int id;
            string name, cat;
            float price;

            cout << "\nEnter Product ID: ";
            cin >> id;

            cout << "Enter Product Name: ";
            cin >> name;

            cout << "Enter Category: ";
            cin >> cat;

            cout << "Enter Price: ";
            cin >> price;

            Product p(id, name, cat, price);
            products.push_back(p);

            cout << "\nProduct Added Successfully!\n";
            break;
        }

        case 2:
            displayAll(products);
            break;

        case 3:
        {
            string cat;
            cout << "\nEnter category to search: ";
            cin >> cat;

            searchCategory(products, cat);
            break;
        }

        case 4:
        {
            int id;
            cout << "\nEnter Product ID to search: ";
            cin >> id;

            searchID(products, id);
            break;
        }

        case 5:
            sortProducts(products);
            break;

        case 6:
            saveProducts(products);
            break;

        case 7:
            cout << "\nExiting Program..." << endl;
            break;

        default:
            cout << "\nInvalid Choice!" << endl;
        }

    } while(choice != 7);

    return 0;
}
