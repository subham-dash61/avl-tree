# avl-tree
#include<stdio.h>
#include<stdlib.h>
// An AVL tree node
struct node
{
int key;
struct node *left,*right;
int height;
};
// A utility function to get maximum of two integers
int max(int a, int b);
// A utility function to get height of the tree
int height(struct node *N)
{
if (N == NULL)
return 0;
return N->height;
}
// A utility function to get maximum of two integers
int max(int a, int b)
{
return (a > b)? a : b;
}
/* Helper function that allocates a new node with the given key and NULL left and right pointers. */
struct node* newNode(int key)
{
struct node *temp = (struct node*)malloc(sizeof(struct node));
temp->key = key;
temp->left = NULL;
temp->right = NULL;
temp->height = 1; // new node is initially
// added at leaf
return(temp);
}
// A utility function to right
// rotate subtree rooted with y
// See the diagram given above.
struct node* rightRotate(struct node *y)
{
struct node *x = y->left;
struct node *T2 = x->right;
// Perform rotation
x->right = y;
y->left = T2;
// Update heights

y
->height = max(height(y
->left),

height(y

->right)) + 1;

x
->height = max(height(x
->left),

height(x

->right)) + 1;

// Return new root
return x;
}
// A utility function to left
// rotate subtree rooted with x
// See the diagram given above.
struct node* leftRotate(struct node *x)
{
struct node *y = x
->right;
struct node *T2 = y
->left;
// Perform rotation
y
->left = x;
x
->right = T2;
// Update heights
x
->height = max(height(x
->left),

height(x

->right)) + 1;

y
->height = max(height(y
->left),

height(y

->right)) + 1;

// Return new root
return y;
}
// Get Balance factor of node N
int getBalance(struct node *N)
{
if (N == NULL)
return 0;
return height(N
->left)
-
height(N
->right);

}
struct node* insert(struct node *temp, int key)
{
/* 1. Perform the normal BST rotation */
if (temp == NULL)
return(newNode(key));
if (key < temp
->key)

temp
->left = insert(temp

->left, key);

else if (key > temp
->key)

temp
->right = insert(temp

->right, key);

else // Equal keys not allowed
return temp;
/* 2. Update height of this ancestor node */
temp
->height = 1 + max(height(temp
->left),

height(temp->right));
/* 3. Get the balance factor of this
ancestor node to check whether
this node became unbalanced */
int balance = getBalance(temp);
// If this node becomes unbalanced,
// then there are 4 cases
// Left Left Case
if (balance > 1 && key < temp->left->key)
return rightRotate(temp);
// Right Right Case
if (balance < -1 && key > temp->right->key)
return leftRotate(temp);
// Left Right Case
if (balance > 1 && key > temp->left->key)
{
temp->left = leftRotate(temp->left);
return rightRotate(temp);
}
// Right Left Case
if (balance < -1 && key < temp->right->key)
{
temp->right = rightRotate(temp->right);
return leftRotate(temp);
}
return temp;
}

struct node* minValueNode(struct node *temp)
{
struct node *current = temp;
/* loop down to find the leftmost leaf */
while (current->left != NULL)
current = current->left;
return current;
}
// Recursive function to delete a node
// with given key from subtree with
// given root. It returns root of the
// modified subtree.
struct node* deleteNode(struct node *root, int key)
{
// STEP 1: PERFORM STANDARD BST DELETE

if (root == NULL)
return root;
// If the key to be deleted is smaller
// than the root's key, then it lies
// in left subtree
if ( key < root->key )
root->left = deleteNode(root->left, key);
// If the key to be deleted is greater
// than the root's key, then it lies
// in right subtree
else if( key > root->key )
root->right = deleteNode(root->right, key);
// if key is same as root's key, then
// This is the node to be deleted
else
{
// node with only one child or no child
if( (root->left == NULL) ||
(root->right == NULL) )
{
struct node *temp = root->left ?
root->left :
root->right;
// No child case
if (temp == NULL)
{
temp = root;
root = NULL;
}
else // One child case
*root = *temp; // Copy the contents of
// the non-empty child
// free(temp);
}
else
{
// node with two children: Get the inorder
// successor (smallest in the right subtree)
struct node *temp = minValueNode(root->right);
// Copy the inorder successor's
// data to this node
root->key = temp->key;
// Delete the inorder successor
root->right = deleteNode(root->right,
temp->key);
}
}
// If the tree had only one node

// then return
if (root == NULL)
return root;
// STEP 2: UPDATE HEIGHT OF THE CURRENT NODE
root->height = 1 + max(height(root->left),
height(root->right));
// STEP 3: GET THE BALANCE FACTOR OF
// THIS NODE (to check whether this
// node became unbalanced)
int balance = getBalance(root);
// If this node becomes unbalanced,
// then there are 4 cases
// Left Left Case
if (balance > 1 &&
getBalance(root->left) >= 0)
return rightRotate(root);
// Left Right Case
if (balance > 1 &&
getBalance(root->left) < 0)
{
root->left = leftRotate(root->left);
return rightRotate(root);
}
// Right Right Case
if (balance < -1 &&
getBalance(root->right) <= 0)
return leftRotate(root);
// Right Left Case
if (balance < -1 &&
getBalance(root->right) > 0)
{
root->right = rightRotate(root->right);
return leftRotate(root);
}
return root;
}
// A utility function to print preorder
// traversal of the tree.
// The function also prints height
// of every node
void preOrder(struct node *root)
{
if(root != NULL)
{
printf("%d ",root->key);
preOrder(root->left);
preOrder(root->right);
}
}
// Driver Code
int main()
{
struct node *root = NULL;
/* Constructing tree given in
the above figure */
root = insert (root, 4);

root = insert (root, 5);

root = insert (root, 144);

root = insert (root, 89);

root = insert (root, 6);

root = insert (root, 45);

root = insert (root, -45);

root = insert (root, 31);

root = insert (root, 72);

printf ("Preorder traversal of the constructed AVL tree is \n");

preOrder (root);

root = deleteNode (root, 144);

root = deleteNode (root, -45);

printf ("\nPreorder traversal after deletion of given node \n");
preOrder(root);
return 0;}
