#include "stdio.h"
#include "string.h"
#include "conio.h"
#include "stdlib.h"

struct Account{
	int zh;
	char name[20];
	int password;
	double money;
	int status;
	int balance;
};
Account user[6];

int order = 1;           //控制顺序的变量
int confirmpassword[6];  //确认用的密码

//菜单1
void ShowMenu_1()
{
	printf("欢迎使用！");
	printf("\t\t主界面\n");
	printf("===========================================\n");
	printf("\t请选择你的操作：\n");
	printf("\t1.开户\t2.登录\t3.退出系统\n");
	printf("\t4.显示目前账户数组信息\n");
	printf("请确认你的选择！\n");
	printf("===========================================\n");
}
//菜单2
void ShowMenu_2()
{
	printf("\t\t欢迎使用ATM银行管理系统,祝您使用愉快！\n");
	printf("===============================================\n");
	printf("\t请选择并确认你的操作：\n");
	printf("\t1.存款\t2.取款\t3.查询\t4.转账\n");
	printf("\t5.挂失\t6.解挂\t7.销户\t8.退出主菜单\n");
	printf("选择完毕！");
	printf("===============================================\n");
}

//开户
void Authoring(Account user[])
{
	user[order].zh = 10000 + order;
	printf("开始注册账户……\n\n");
	printf("请输入用户姓名：");
	scanf("%s",&user[order].name);
	printf("请输入密码：");
	scanf("%d",&user[order].password);
	printf("请再次输入密码确认：");
	scanf("%d",&confirmpassword[order]);

	if(user[order].password == confirmpassword[order])
	{
		printf("开户成功！\n");
		printf("您的账号是：%d\n\n",user[order].zh);
		order++;
	}
	else
	{
		printf("密码不一致，开户失败！\n");
		user[order].password = 0;
		confirmpassword[order] = 0;
	}
}
//存款
double Desposit(Account user[],int a)
{
	double desposit_money = 0;
	printf("请输入您要存储的金额为：");
	scanf("%lf",&desposit_money);

	return user[a].money += desposit_money;
}

//取款
double Draw(Account user[],int b)
{
	double draw_money = 0;
	printf("请输入您要取出的金额为：");
	scanf("%lf",&draw_money);

	if(draw_money < user[b].money)
	{
		return user[b].money -= draw_money;
	}
	else
	{
		printf("对不起，余额不足，无法取出，请检查账户余额，然后重试，谢谢\n\n");
		return -1;
	}
}

//转账0
void Transfer(Account user[],int c)
{
	Account target;
	printf("请输入您要转账的对象账号为：");
	scanf("%d",&target.zh);

	for(int j = 1;j <= 6;j++)
	{
		if(target.zh == user[j].zh)
		{
			double transfer_money = 0;
			printf("请输入您要转账的金额为：");
			scanf("%lf",&transfer_money);

			if(transfer_money < user[c].money)
			{
				user[j].money += transfer_money;
				user[c].money -= transfer_money;
				break;
			}
			else
			{
				printf("对不起，余额不足！\n\n");
				break;
			}
		}
	}

	if(target.zh != user[j].zh)
	{
		printf("对不起，没有找到你要转账的对象！\n\n");
		printf("请重试！");
	}
}
void GuaShi(Account user[],int h)
{
	printf("户主姓名为：%s\n",user[h].name);
    if(user[h].status == 0)
	{
	   user[h].status = 1;
	   printf("挂失成功！\n");
	}
}


void JieGua(Account user[],int m)
{
    printf("户主姓名为：%s\n",user[m].name);
	if(user[m].status == 0)
	{
		printf("该账户处于正常状态，无需挂失！\n");
	}
	else if(user[m].status == 1)
	{
		user[m].status = 0;
		printf("解除挂失成功！\n");
	}
}
void XiaoHu(Account user[],int n)
{
	printf("户主姓名，%s\t余额：%.2f元\n",user[n].name,user[n].balance);

	user[n].balance = 0;
	user[n].status = 2;
    printf("取款: %.2f元，销户成功！\n",user[n].balance);

}




//用户界面
int ATMsystem(Account user[],int p)
{
	int q;

	while(1)
	{
		ShowMenu_2();

		printf("请选择相应菜单项：");
		scanf("%d",&q);
		printf("\n\n");

		switch(q)
		{
		case 1:{
			Desposit(user,p);
			printf("当前余额为：%.2lf\n\n",user[p].money);
			break;
			   }

		case 2:{
			Draw(user,p);
			printf("当前余额为：%.2lf\n\n",user[p].money);
			break;
			   }

		case 3:{
			printf("当前余额为：%.2lf\n\n",user[p].money);
			break;
			   }

		case 4:{
			Transfer(user,p);
			printf("当前余额为：%.2lf\n\n",user[p].money);
			break;
			   }

		case 5:
			GuaShi(user,p);
			break;
             
		case 6:
			JieGua(user,p);
			break;
		case 7:
			XiaoHu(user,p);
			break;
		case 8:{
			printf("正在退出……\n\n\n\n\n\n");
			return q;
			   }
		}
	}
}

//登录
void Enroll(Account user[])
{
	Account user2;
	char mm[10];
	int pass;

	printf("请输入你的账号：");
	scanf("%d",&user2.zh);

	printf("请输入密码并确认：");
	for(int i = 0;i < 10;i++)
	{
			mm[i] = getch();
			if(mm[i] == '\r')
				break;
			if(mm[i] >= '0' && mm[i] <= '9')
			{
				printf("*");
			}
	}
	mm[i] = '\0';
	pass = atoi(mm);

	printf("\n\n");

	for(i = 1;i <= 6;i++)
	{
		if(user2.zh == user[i].zh)
		{
			if(pass == user[i].password)
			{
				printf("\n\n\n\n\n\n");
				ATMsystem(user,i);
				break;
			}
			else
			{
				printf("账号或密码错误！请检查并重新输入\n\n");
				break;
			}
		}
	}
	
}
//显示目前账户数组信息
void Show_accounts(Account user[])
{
	printf("账号数组内容如下：\n");
	for(int t = 1;t <= 6;t++)
	{
		printf("账户为：%d\t",user[t].zh);
		printf("用户名为：%s\t",user[t].name);
		printf("密码为：%d\t",user[t].password);
		printf("余额为：%.2lf\t",user[t].money);
		printf("状态为：%s\n",user[t].status);
	}

	printf("\n\n");
}

int main()
{
	int h;

	memset(user,0,sizeof(user));

	while(1)
	{
		ShowMenu_1();

		printf("请选择相应菜单项：");
		scanf("%d",&h);
		printf("\n\n");

		switch(h)
		{
		case 1:Authoring(user);
			break;

		case 2:Enroll(user);
			break;

		case 3:printf("感谢使用，再见！\n");
			return 0;

		case 4:Show_accounts(user);
			break;
		}
	}
}
