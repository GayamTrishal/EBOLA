# EBOLA
# PROBLEM STATEMENT

An analysis of the ongoing Ebola outbreak reveals that transmission of the virus occurs in social networks. Usually a social network is represented as an undirected graph G = (V, E), where V denotes the set of nodes and E denotes the set of edges. Each node in the graph is assigned an integer weight for it.

At time t = 0, all the nodes of the graph are vulnerable. Node S of the graph has just been infected by Ebola. At each instant of time t > 0, any vulnerable node v which is connected to some infected node u (i.e. (u, v) ∈ E) also gets infected, unless it was vaccinated at some time t' <= t. A node once infected always remains infected and a vaccinated node can never be infected. Obviously once a node is infected, there is no point of vaccinating it. A node is called saved if it is not infected till the end (at t = infinity, a long time afterwards).

You, being in-charge of biggest hospital in the area, have been given the responsibility of vaccinating the nodes of the graph. There are total K units of supply of vaccines in the hosptial. Your target is to vaccinate certain number of nodes using these K units of vaccines. You will start vaccinating the nodes from time t = 1, up to time t = K. Note that you can not vaccinate more than one node a particular time. Also notice that it is better to not skip a vaccination at any time instance between t = 1 to t = K, so we enforce you to vaccinate at least some node at each instant. You can choose to vaccinate a node more than once, but it will be pointless. You can also choose to try to vaccinate an already infected node, but it will also be pointless.
What needs to be optimized?

Your score will be the sum of weights of all the nodes those are not infected (at t = inf, after a long time). You have to maximize value of score.
Input

The first line contains four space separated integers N, M, K and S, denoting the number of nodes, the number of edges,the number of units of the vaccines available and the index of the node at which the Ebola virus starts infecting the social network respectively.

The next line contains N space separated integers wi , each integer specifying the weight of ith node, where 1 ≤ i ≤ N.

Then M lines follow.

The ith of such lines contains two space separated integers x and y representing the edge joining nodes x and y.
Output

Output K space separated integers denoting the indices of the nodes in the order of their vaccination. Note that if you don't have any uninfected node to vaccinate, you can print any dummy node to try to vaccinate.
Constraints

    1 ≤ S ≤ N ≤ 104
    1 ≤ M ≤ 5*104
    1 ≤ K ≤ 500
    1 ≤ wi ≤ 103
    Graph G is connected. It does not have multi-edges and self loops.

Example

Input:

    15 22 3 1
    2 3 1 4 5 3 6 8 2 4 8 1 4 8 9
    1 2
    1 7
    1 12
    2 3
    2 4
    2 5
    2 6
    3 4
    3 9
    4 6
    5 6
    5 8
    5 10
    6 10
    7 8
    7 11
    8 10
    9 14
    12 13
    12 14
    12 15
    14 15

Output:

    2 14 6

Explanation

The above example is well illustrated through following figures. Infection is represented by red circle and vaccination is represented by green circle. At the end of the process nodes 2, 3, 4, 6, 9 and 14 are saved. The sum of the weights of these nodes is 21. So, the score is 21.

t = 0

t = 1

t = 2

t = 3
Test Case Generation

All the test cases have been divided into broadly 4 groups.
Description of groups of test cases

Each group has 5 test files. All the test files in the group will follow these constraints.

    minN ≤ N ≤ maxN
    minM ≤ M ≤ maxM
    minK ≤ K ≤ maxK
    N, M, K will be choosen uniformly randomly in the above described ranges.
    S will choosen uniformly randomly in the range [1..N].

In one test file, Graph G will be a 4-regular graph. Please ignore the constraints on M displayed above in that case. All other constraints will be applicable on G.

In one of the test files, the graph will be a connected biparite graph following the given constraints.

In other remaining three files, the graph will be random connected graphs satisfying the given constraints.

Group 1

    minN = 290 & maxN = 300
    minM = 900 & maxM = 1000
    minK = 35 & maxK = 50

Group 2

    minN = 970 & maxN = 1000
    minM = 9900 & maxM = 10000
    minK = 35 & maxK = 50

Group 3

    minN = 9990 & maxN = 10000
    minM = 35000 & maxM = 40000
    minK = 35 & maxK = 50

Group 4

    minN = 9990 & maxN = 10000
    minM = 35000 & maxM = 40000
    minK = 350 & maxK = 500

During the contest, your submissions will be evaluated on the test files. If you get a non-AC verdict in any of the test files, you will be notified about that. However, if you get an AC verdict, then the score displayed will be only of 20% of the test files. Those 20% of the test files will be 4-regular, bipartite and 2 random connected graphs of group 4. After the contest, your submission will be rejudged and score will correspond to all the test files.

******************************************************************************************************************************

#SOLUTION

    #include <iostream>
 
    using namespace std;
    long long int v[10001],w[10001];
    struct node{
             long long int val;
             node *right,*left;
            };
    class adj_list{
                    node *front,*rear;
                public:
                    void push(long long int);
                    void adj();
                    bool check();
                    long long int find_max();
                    void clear();
                    adj_list()
                    {
                        front=NULL;
                        rear=NULL;
                    }
                }y;
void adj_list::push(long long int x)
{
    node *add;
    add=new struct node;
    add->val=x;
    add->right=NULL;
    add->left=NULL;
    if(front==NULL)
    {
        front=add;
        rear=add;
    }
    else
    {
        add->left=rear;
        rear->right=add;
        rear=add;
    }
}
void adj_list::adj()
{
    if(front==NULL)
        return;
    else
    {
        node *ptr;
        ptr=front;
        while(ptr!=rear)
        {
            if(v[ptr->val]==0)
                y.push(ptr->val);
            ptr=ptr->right;
        }
        if(v[ptr->val]==0)
            y.push(ptr->val);
    }
}
bool adj_list::check()
{
    if(front==NULL)
        return false;
    else
        return true;
}
long long int adj_list::find_max()
{
    node *ptr;
    long long int max=front->val;
    v[max]=2;
    ptr=front;
    while(ptr!=rear)
    {
        if(w[max]<w[ptr->val])
        {
            v[max]=1;
            max=ptr->val;
            v[max]=2;
        }
        else
            v[ptr->val]=1;
        ptr=ptr->right;
    }
    if(w[max]<w[ptr->val])
    {
        v[max]=1;
        max=ptr->val;
        v[max]=2;
    }
    else
        v[ptr->val]=1;
    return max;
}
void adj_list::clear()
{
    node *ptr;
    ptr=front;
    while(ptr!=rear)
    {
        node *del;
        del=ptr;
        ptr=ptr->right;
        delete del;
    }
    delete ptr;
    front=NULL;
    rear=NULL;
}
int main()
{
    long long int n,m,s,k;
    cin>>n>>m>>k>>s;
    long long int i,u,z,p,j,max2;
    w[0]=0;
    adj_list x[n+1];
    for(i=0;i<=n;i++)
        v[i]=0;
    for(i=1;i<=n;i++)
        cin>>w[i];
    for(i=0;i<m;i++)
    {
        cin>>u>>z;
        x[u].push(z);
        x[z].push(u);
    }
    v[s]=1;
    for(j=0;j<k;j++)
    {
        for(i=1;i<=n;i++)
        {
            if(v[i]==1)
                x[i].adj();
        }
        if(y.check())
        {
            max2=0;
            p=y.find_max();
            for(i=1;i<=n;i++)
            {
                if(v[i]==0)
                {
                    if(w[max2]<w[i])
                        max2=i;
                }
            }
            if(w[p]<w[max2])
            {
                v[p]=1;
                v[max2]=2;
                p=max2;
            }
            cout<<p<<"\n";
            y.clear();
        }
        else
            break;
    }
    k=k-j;
    long long int max,count=0;
    for(i=1;i<=n;i++)
    {
        if(v[i]==0)
            count++;
    }
    if(k>0)
    {
    if(count<=k)
    {
        for(i=1;i<=n;i++)
        {
            if(v[i]==0)
                cout<<i<<"\n";
        }
        k-=count;
        for(i=1;(i<=n)&&(k>0);i++)
        {
            if(v[i]==1)
            {
                cout<<i<<"\n";
                k--;
            }
        }
    }
    else
    {
        while(k--)
        {
            max=0;
            for(i=1;i<=n;i++)
            {
                if(v[i]==0)
                {
                    if(w[max]<w[i])
                        max=i;
                }
            }
            cout<<max<<"\n";
            v[max]=2;
            if(k==0)
                break;
        }
    }
    }
    return 0;
}
