# 银行排队问题

## 题目解析
这题和之前的14题目很类似，我用的所有窗口共享同一个时间刷新率，就比较暴力的按照秒来，一次增加一秒（也可以不共享时间，这样可能时间开销上会小一些）。按照题目的要求应该是所有在17点之前到的用户都必须全部服务完成即使时间过了17点（这点和14题不同）。[题目链接](https://pintia.cn/problem-sets/994805342720868352/problems/994805491530579968)

## 具体代码(C++)
```c++
//提交时间	状态	分数	题目	编译器	耗时	用户
//2019/10/20 00:05:54	
//答案正确
//25	1017	C++ (g++)	18 ms（只有最后一个测试点是18ms，其他的都是3-4ms）
#include <iostream>
#include <vector>
#include <cstdio>
#include <queue>
using namespace std;

struct per_cus{
    int hour,minitus,second;
    int time_need;
    int total_sec;
    int wait_time;
    int time_start;
};
bool operator<(const per_cus& a,const per_cus& b)
{
    return a.total_sec>b.total_sec;
}

class Solution{
public:
    int customer_num,window_num;
    priority_queue<per_cus> cus_list;
    Solution()
    {
        cin>>customer_num>>window_num;
        for(int i=0;i<customer_num;i++)
        {
            per_cus item;
            scanf("%d:%d:%d %d",&item.hour,&item.minitus,&item.second,&item.time_need);
            item.total_sec=item.hour*3600+item.minitus*60+item.second;
            if(item.total_sec<61201)
                cus_list.push(item);
        }
    }
    void get_res()
    {
        vector<queue<per_cus> > windows;
        windows.resize(window_num);
        float cus_serverd=(float)cus_list.size(),total_wait_time=0;
        int t=8*3600;
        while(!cus_list.empty())
        {
            for(int i=0;i<window_num;i++)
            {
                if(!windows[i].empty())
                {
                    per_cus item=windows[i].front();
                    if(item.time_start+item.time_need*60==t)
                        windows[i].pop();
                }
                if(windows[i].empty())
                {
                    per_cus top=cus_list.top();
                    if(t>=top.total_sec)
                    {
                        top.time_start=t;
                        top.wait_time=t-top.total_sec;
                        cus_list.pop();
                        total_wait_time+=top.wait_time;
                        windows[i].push(top);
                    }
                }
            }
            t++;
        }
        if(cus_serverd==0) {
            printf("0.0");
            return;
        }
        float res=(total_wait_time/cus_serverd)/60.0;
        printf("%.1f",res);
    }

};


int main()
{
    Solution s=Solution();
    s.get_res();
    return 0;
}
```