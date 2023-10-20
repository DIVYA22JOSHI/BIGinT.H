#include<stdio.h>
#include<stdlib.h>

typedef struct node{
    int data;
    struct node*next;
}node;

typedef struct {
    node*head;
    char sign;
    long int size;
}BIG_INT;

BIG_INT* initialize();
void add_node(int element,BIG_INT* num);
BIG_INT* get_num();
void display(BIG_INT *num);
//void remove_trailing_zero(BIG_INT*num);
long int length(node*head);
void insert_tail(BIG_INT*num,int element);
BIG_INT* add(BIG_INT* num1, BIG_INT* num2);
BIG_INT* multiply(BIG_INT *n1,BIG_INT*n2);
BIG_INT*subtract(BIG_INT* num1, BIG_INT* num2);
BIG_INT*division(BIG_INT*num1,BIG_INT*num2);
int compare(BIG_INT*num1,BIG_INT*num2);
void free_number(BIG_INT*num)
{
    node*itr=num->head;
    while(itr!=NULL)
    {
        node*temp=itr;
        itr=itr->next;
        free(temp);
    }
    free(num);
}
BIG_INT* create_num(char *str)
{
    BIG_INT*num=(BIG_INT*)malloc(sizeof(BIG_INT));
    if(num==NULL)
    {
        printf("memory allocation failed\n");
        exit(1);
    }
    num->head=NULL;
    int i=0;
    if (str[0] == '-') {
        num->sign='-';
        i++;
    }
    else{
        num->sign='+';
    }

    while (str[i] != '\0') {

        if (str[i] >= '0' && str[i] <= '9') {
            int digit = str[i] - '0';
            add_node(digit, num);
        }
        else {
        printf("enter a valid digit\n");
        exit(1);
        }
        i++;
    }

    return num;
}
node* reverse(node*head)
{
    node*back=NULL,*front=NULL,*current=head;
    while(current!=NULL)
    {
        front=current->next;
        current->next=back;
        back=current;
        current=front;
    }
    return back;
}
void main()
{
    BIG_INT *num1,*num2,*num3;
    num3=initialize();
    printf("enter first number\n");
    num1=get_num();
   // printf("size: %ld\n",num1->size);

   printf("enter second number\n");
   num2=get_num();
    //num2=create_num("-100");
   // printf("size: %ld\n",num2->size);
   // printf("%d",compare(num1,num2));
   printf("addition: ");
    num3=add(num1,num2);
   // printf("\nsize: %ld\n",num3->size);
    display(num3);
    free_number(num3);
    printf("\n");
    printf("subtraction: ");
    num3=subtract(num1,num2);
   // printf("\nsize: %ld\n",num3->size);
    display(num3);
    printf("\n");
    printf("multiplicaton: ");
    num3=multiply(num1,num2);
  //  printf("\nsize: %ld\n",num3->size);
    display(num3);
    printf("\n");
    display(num1);
    printf("\n");
    display(num2);



}
void insert_tail(BIG_INT*num,int element)
{
    node *temp=(node*)malloc(sizeof(node));
    if(temp==NULL)
    {
        printf("memory allocation failed\n");
        return;
    }
    temp->data=element;
    temp->next=NULL;
    num->size+=1;
    if(num->head==NULL)
    {
        num->head=temp;
        return;
    }
    node*itr=num->head;
    while(itr->next!=NULL)
    {
        itr=itr->next;
    }
    itr->next=temp;

}
long int length(node*head)
{
    long int count=0;
    while(head)
    {
        head=head->next;
        count++;
    }
    return count;
}

BIG_INT* initialize()
{
    BIG_INT*num=(BIG_INT*)malloc(sizeof(BIG_INT));
    if(num==NULL)
    {
        printf("memory allocation failed\n");
        exit(1);
    }
    num->head=NULL;
    num->sign='+';
    num->size=0;
    return num;
}

void add_node(int element,BIG_INT* num)
{
    node*temp=(node*)malloc(sizeof(node));
    if(temp==NULL)
    {
        printf("memory allocation failed\n");
        exit(1);
    }
    temp->data=element;
    temp->next=num->head;
    num->head=temp;
    num->size+=1;
}

BIG_INT* get_num()
{
    BIG_INT*num=(BIG_INT*)malloc(sizeof(BIG_INT));
    if(num==NULL)
    {
        printf("memory allocation failed\n");
        exit(1);
    }
    num->head=NULL;
    char c;
    if ((c = getchar()) == '-') {
        num->sign='-';
        c = getchar();
    }
    else{
        num->sign='+';
    }
    while (c != '\n') {
        if (c >= '0' && c <= '9') {
            int digit = c - '0';
            add_node(digit, num);
        }
        else {
        printf("enter a valid digit\n");
        exit(1);
        }
        c = getchar();
    }

    return num;
}
void display(BIG_INT *num)
{
    if(num->sign=='-')
    {
        printf("%c",'-');
    }
    if(num==NULL || num->head==NULL)
    {
        return;
    }
   void display_reverse(node*head)
   {
        if(head==NULL)
        {
         return;
        }
        display_reverse(head->next);
        printf("%d",head->data);
   }
   display_reverse(num->head);

}
BIG_INT* add(BIG_INT* num1, BIG_INT* num2){
    BIG_INT*num3=initialize();

    if(num1->sign=='-' && num2->sign=='-')
    {
        num3->sign='-';
    }
    else if(num1->sign=='-' )
    {
      num1->sign='+';
      num3=subtract(num2,num1);
      num1->sign='-';
      return num3;
    }
    else if(num2->sign=='-')
    {
      num2->sign='+';
      num3=subtract(num1,num2);
      num2->sign='-';
      return num3;
    }
    node*l1=num1->head,*l2=num2->head;
    int sum=0;
    int carry=0;
    while(l1!=NULL && l2!=NULL)
    {
        sum=l1->data+l2->data+carry;
        carry=sum/10;
        sum=sum%10;
        insert_tail(num3,sum);
        l1=l1->next;
        l2=l2->next;
    }
   while(l1!=NULL)
    {
        sum=l1->data+carry;
        carry=sum/10;
        sum=sum%10;
        insert_tail(num3,sum);
        l1=l1->next;
    }
     while(l2!=NULL)
    {
        sum=l2->data+carry;
        carry=sum/10;
        sum=sum%10;
        insert_tail(num3,sum);
        l2=l2->next;
    }
    if(carry!=0)
    {
        insert_tail(num3,carry);
    }
    return num3;

}
BIG_INT* multiply(BIG_INT *n1,BIG_INT*n2)
{
    node*num1=n1->head,*num2=n2->head;
    if(num1==NULL || num2==NULL)
    {
        return NULL;
    }
    BIG_INT*result=initialize(),*mid;
    int carry=0,product=0,i=0;
    while(num2!=NULL)
    {
        node*num1=n1->head;
        carry=0;
        mid=initialize();
        for(int j=0;j<i;j++)
        {
            insert_tail(mid,0);
        }
        while(num1!=NULL)
        {
            product=(num1->data)*(num2->data)+carry;
            insert_tail(mid,product%10);
            carry=product/10;
            num1=num1->next;
        }

        if(carry>0)
        {
             insert_tail(mid,carry);
        }

        result=add(mid,result);
        node* current = mid->head;
        free_number(mid);
       /* while (current != NULL) {
            node* next = current->next;
            free(current);
            current = next;
        }
        free(mid);*/
        num2=num2->next;
        i++;
    }
    if(n1->sign!=n2->sign)
    {
        result->sign='-';
    }
    return result;
}

BIG_INT*subtract(BIG_INT* num1, BIG_INT* num2){

    node*l1=num1->head,*l2=num2->head;
    BIG_INT *num3=initialize();
    if(num1->sign=='+' && num2->sign=='-')
    {
        num2->sign='+';
        num3=add(num1,num2);
        num2->sign='-';
        num3->sign='+';
        return num3;
    }
    else if(num1->sign=='-'&& num2->sign=='+' )
    {
      num1->sign='+';
      num3=add(num2,num1);
      num1->sign='-';
      num3->sign='-';
      return num3;
    }

    //printf("%d",compare(num1,num2));
     if(compare(num1,num2))
    {
        node*temp=l1;
        l1=l2;
        l2=temp;
        num3->sign='-';
    }

    int sub=0;
    int borrow=0;
    while(l1!=NULL && l2!=NULL)
    {
        sub=l1->data-l2->data+borrow;
        if(sub<0)
        {
            borrow=-1;
            sub=sub+10;
        }
        else
        {
            borrow=0;
        }
        insert_tail(num3,sub);
        l1=l1->next;
        l2=l2->next;
    }
   while(l1!=NULL)
    {
        sub=l1->data+borrow;
        borrow=0;
        insert_tail(num3,sub);
        l1=l1->next;
    }
   //remove_trailing_zero(num3);
    return num3;
}
int compare(BIG_INT*num1,BIG_INT*num2)
{

    //it would return 1 if n2 is greater otherwise 0
    if(num2->size>num1->size)
    {
        return 1;
    }
    else if(num2->size==num1->size)
    {
        int return_val=0;
        num1->head= reverse(num1->head);
        num2->head= reverse(num2->head);
        node*head1=num1->head,*head2=num2->head;
        while(head1!=NULL && head2!=NULL)
        {
            if(head2->data>head1->data)
            {
                return_val= 1;
                break;
            }
            else if(head1->data<head2->data)
            {
                return_val= 0;
                break;
            }
            head1=head1->next;
            head2=head2->next;
        }
        num1->head= reverse(num1->head);
        num2->head= reverse(num2->head);
        return return_val;
    }
    return 0;
}
BIG_INT*division(BIG_INT*num1,BIG_INT*num2)
{
  //  node*head1=num1->head,*head2=num2->head;

}






