import java.util.*;
class solution {
 
    static class Node {
        Node left, right;
        int info;

        boolean lthread;

        boolean rthread;
    };
 
    static Node insert(Node root, int ikey)
    {
        Node ptr = root;
        Node par = null; // Parent of key to be inserted
        while (ptr != null) {
            // If key already exists, return
            if (ikey == (ptr.info)) {
                System.out.printf("Duplicate Key !\n");
                return root;
            }
 
            par = ptr; 
            if (ikey < ptr.info) {
                if (ptr.lthread == false)
                    ptr = ptr.left;
                else
                    break;
            }
            else {
                if (ptr.rthread == false)
                    ptr = ptr.right;
                else
                    break;
            }
        }
        Node tmp = new Node();
        tmp.info = ikey;
        tmp.lthread = true;
        tmp.rthread = true;
 
        if (par == null) {
            root = tmp;
            tmp.left = null;
            tmp.right = null;
        }
        else if (ikey < (par.info)) {
            tmp.left = par.left;
            tmp.right = par;
            par.lthread = false;
            par.left = tmp;
        }
        else {
            tmp.left = par;
            tmp.right = par.right;
            par.rthread = false;
            par.right = tmp;
        }
 
        return root;
    }
    static Node inSucc(Node ptr)
    {
        if (ptr.rthread == true)
            return ptr.right;
 
        ptr = ptr.right;
        while (ptr.lthread == false)
            ptr = ptr.left;
 
        return ptr;
    }
    static Node inorderSuccessor(Node ptr)
    {
        if (ptr.rthread == true)
            return ptr.right;
        ptr = ptr.right;
        while (ptr.lthread == false)
            ptr = ptr.left;
        return ptr;
    }
    static void inorder(Node root)
    {
        if (root == null)
            System.out.printf("Tree is empty");
        Node ptr = root;
        while (ptr.lthread == false)
            ptr = ptr.left;
        while (ptr != null) {
            System.out.printf("%d ", ptr.info);
            ptr = inorderSuccessor(ptr);
        }
    }
 
    static Node inPred(Node ptr)
    {
        if (ptr.lthread == true)
            return ptr.left;
 
        ptr = ptr.left;
        while (ptr.rthread == false)
            ptr = ptr.right;
        return ptr;
      
    }
    static Node caseA(Node root, Node par,
                      Node ptr)
    {
        if (par == null)
            root = null;
        else if (ptr == par.left) {
            par.lthread = true;
            par.left = ptr.left;
        }
        else {
            par.rthread = true;
            par.right = ptr.right;
        }
 
        return root;
    }
 
    static Node caseB(Node root, Node par,
                      Node ptr)
    {
        Node child;
        if (ptr.lthread == false)
            child = ptr.left;
        else
            child = ptr.right;
        if (par == null)
            root = child;
        else if (ptr == par.left)
            par.left = child;
        else
            par.right = child;
        Node s = inSucc(ptr);
        Node p = inPred(ptr);
 
        if (ptr.lthread == false)
            p.right = s;
        else {
            if (ptr.rthread == false)
                s.left = p;
        }
 
        return root;
    }
    static Node caseC(Node root, Node par,
                      Node ptr)
    {
        Node parsucc = ptr;
        Node succ = ptr.right;

        while (succ.lthread == false) {
            parsucc = succ;
            succ = succ.left;
        }
 
        ptr.info = succ.info;
 
        if (succ.lthread == true && succ.rthread == true)
            root = caseA(root, parsucc, succ);
        else
            root = caseB(root, parsucc, succ);
 
        return root;
    }
    static Node delThreadedBST(Node root, int dkey)
    {
        Node par = null, ptr = root;
        int found = 0;
        while (ptr != null) {
            if (dkey == ptr.info) {
                found = 1;
                break;
            }
            par = ptr;
            if (dkey < ptr.info) {
                if (ptr.lthread == false)
                    ptr = ptr.left;
                else
                    break;
            }
            else {
                if (ptr.rthread == false)
                    ptr = ptr.right;
                else
                    break;
            }
        }
 
        if (found == 0)
            System.out.printf("dkey not present in tree\n");
        else if (ptr.lthread == false && ptr.rthread == false)
            root = caseC(root, par, ptr);
        else if (ptr.lthread == false)
            root = caseB(root, par, ptr);
        else if (ptr.rthread == false)
            root = caseB(root, par, ptr);
        else
            root = caseA(root, par, ptr);
 
        return root;
    }
 
    public static void main(String args[])
    {
        Node root = null;
 
        root = insert(root, 20);
        root = insert(root, 10);
        root = insert(root, 30);
        root = insert(root, 5);
        root = insert(root, 16);
        root = insert(root, 14);
        root = insert(root, 17);
        root = insert(root, 13);
 
        root = delThreadedBST(root, 20);
        inorder(root);
    }
}
