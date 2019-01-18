#define _CRT_SECURE_NO_WARNINGS
/* ************************************************************************ */
/* 示例代码维护：哈尔滨理工大学 计算机博弈研究组							*/
/* 己方棋子编码约定:														*/
/*	a司令,b军长,c师长,d旅长,e团长,f营长,g连长,h排长,i工兵,j地雷k炸弹,l军旗	*/
/* 对方方棋子编码约定:														*/
/*	A司令,B军长,C师长,D旅长,E团长,F营长,G连长,H排长,I工兵,J地雷K炸弹,L军旗	*/
/*	X未知对方棋子,0空棋位													*/
/* 最后更新：2012-03-31  meixian@hrbust.edu.cn								*/
/* ************************************************************************ */
#include <stdio.h>
#include <windows.h>
#include <iostream>
#include <string.h>
using namespace std;
/* ************************************************************************ */
/* 函数功能：i,j位置是否本方棋子											*/
/* 接口参数：																*/
/*     char cMap[12][5] 棋盘局面											*/
/*     int i,j 棋盘位置行列号												*/
/* 返回值：																	*/
/*     1己方棋子，0空棋位或对方棋子											*/
/* ************************************************************************ */
int IsMyChess(char cMap[12][5],int i,int j)
{
	if(cMap[i][j]>='a'&& cMap[i][j]<='l')
		return 1;
	else
		return 0;
}

/* ************************************************************************ */
/* 函数功能：i,j位置是否本方可移动的棋子									*/
/* 接口参数：																*/
/*     char cMap[12][5] 棋盘局面											*/
/*     int i,j 棋盘位置行列号												*/
/* 返回值：																	*/
/*     1己方可移动棋子(司令,军长,...,工兵,炸弹)，0军旗,地雷,对方棋子或空棋位*/
/* ************************************************************************ */
int IsMyMovingChess(char cMap[12][5],int i,int j)
{
	if(cMap[i][j]>='a' && cMap[i][j]<='i' || cMap[i][j]=='k')
		return 1;
	else
		return 0;
}

/* ************************************************************************ */
/* 函数功能：i,j位置是否山界后的兵站															*/
/* 接口参数：																							*/
/*     int i,j 棋盘位置行列号																			*/
/* 返回值：																								*/
/*     1处于山界后，0不处于山界后																*/
/* ************************************************************************ */
int IsAfterHill(int i,int j)
{
	if(i*5+j==31 || i*5+j==33)
		return 1;
	else
		return 0;
}
/* ************************************************************************ */
/* 函数功能：i,j位置是否是对手棋子											*/
/* 接口参数：																*/
/*     char cMap[12][5]	棋盘局面											*/
/*     int i, j	棋盘位置行列号												*/
/* 返回值：																	*/
/*     1对手棋子，0空棋位或己方棋子											*/
/* ************************************************************************ */
int IsOppChess(char cMap[12][5],int i,int j)
{
	if(cMap[i][j]>='A' && cMap[i][j]<='X')
		return 1;
	else
		return 0;
}

//函数功能：i,j位置是否山界前的兵站
int IsBehindHill(int i,int j)
{
	if(i*5+j==26 || i*5+j==28)
		return 1;
	else
		return 0;
}
/* ************************************************************************ */
/* 函数功能：i,j位置是否行营												*/
/* 接口参数：																*/
/*     int i,j 棋盘位置行列号												*/
/* 返回值：																	*/
/*     1是行营，0不是行营													*/
/* ************************************************************************ */
int IsMoveCamp(int i,int j)
{
	if(i*5+j==11 || i*5+j==13 || i*5+j==17 || i*5+j==21 || i*5+j==23 || i*5+j==36 || i*5+j==38 || i*5+j==42 || i*5+j==46 || i*5+j==48)
		return 1;
	else
		return 0;
}

/* ************************************************************************ */
/* 函数功能：i,j位置是否大本营											*/
/* 接口参数：																*/
/*     int i,j 棋盘位置行列号												*/
/* 返回值：																	*/
/*     1是大本营，0不是大本营												*/
/* ************************************************************************ */
int IsBaseCamp(int i,int j)
{
	if(i*5+j==1 || i*5+j==3 || i*5+j==56 || i*5+j==58)
		return 1;
	else
		return 0;
}

/* ************************************************************************ */
/* 函数功能：i,j位置是否有棋子占位的行营										*/
/* 接口参数：																*/
/*     char cMap[12][5] 棋盘局面											*/
/*     int i,j 棋盘位置行列号												*/
/* 返回值：																	*/
/*     1有棋子占位的行营,0不是行营或是空行营								*/
/* ************************************************************************ */
int IsFilledCamp(char cMap[12][5],int i,int j)
{
	if(IsMoveCamp(i,j) && cMap[i][j]!='0')
		return 1;
	else
		return 0;
}


//函数功能：判断旗子是否在铁轨上
//int int （i，j）为棋盘位置行列号
int IsRailway(int i,int j)
{
	if(i==1 || i==5 || i==6 || i==10 || ((j==0 || j==4) && i>0 && i<11))
		return 1;
	else
		return 0;
}

int Iszuoqian(int i,int j)
{
	if(i*5+j==19 || i*5+j==29 || i*5+j==27 || i*5+j==44 || i*5+j==52 || i*5+j==54)
		return 1;
	else
		return 0;
}

//(i，j)为不在行营且可以向右前方移动的旗子
//  1为是那三点，0不是那三点         
int Isyouqian(int i,int j)
{
	if(i*5+j==15 || i*5+j==25 || i*5+j==27 || i*5+j==40 || i*5+j==50 || i*5+j==52)
		return 1;
	else
		return 0;
}
//(i，j)为不在行营且可以向左后方移动的旗子
//          1为是那三点，0不是那三点        

int Iszuohou(int i,int j)
{
	if(i*5+j==7 || i*5+j==9 || i*5+j==19 || i*5+j==32 || i*5+j==34 || i*5+j==44)
		return 1;
	else
		return 0;
}
//(i，j)为不在行营且可以向右后方移动的旗子
//         1为是那三点，0不是那三点        
int Isyouhou(int i,int j)
{
	if(i*5+j==5 || i*5+j==7 || i*5+j==15 || i*5+j==30 || i*5+j==32 || i*5+j==40)
		return 1;
	else 
		return 0;
}
/* ************************************************************************ */
/* 函数功能：双方布局后棋局初始化											*/
/* 接口参数：																*/
/*     char cMap[12][5] 棋盘局面											*/
/*     char *cOutMessage 布局字符序列										*/
/* ************************************************************************ */
void InitMap(char cMap[12][5],char *cOutMessage)
{
	int i,j,k;
	for(i=0;i<6;i++)	//标记对方棋子
		for(j=0;j<5;j++)
			if(IsMoveCamp(i,j))
				cMap[i][j]='0';
			else
				cMap[i][j]='X';
	k=6;
	for(i=6;i<12;i++)	//标记己方棋子
		for(j=0;j<5;j++)
			if(IsMoveCamp(i,j))
				cMap[i][j]='0';
			else
				cMap[i][j]=cOutMessage[k++];

}

/* ************************************************************************ */
/* 函数功能：根据裁判反馈刷新棋盘											*/
/* 接口参数：																*/
/*     char cMap[12][5] 棋盘局面											*/
/*     char *cInMessage 来自裁判的GO YXYX R YX命令							*/
/*     char *cOutMessage 发给裁判的BESTMOVE YXYX命令						*/
/* ************************************************************************ */
void FreshMap(char cMap[12][5],char *cInMessage,char *cOutMessage)
{
	int x1,y1;				//起点																										//x1,y1,x2,y2,result由char型改为int型，没什么影响
	int x2,y2;				//落点
	int result=-1;			//碰子结果		
    if(cInMessage[0]=='G')	// GO 指令
	{
		if(cInMessage[3]>='A' && cInMessage[3]<='L')																					//x,y坐标发生对换，因为不习惯
		{
			y1=cInMessage[3]-'A';		
			x1=cInMessage[4]-'0';
			y2=cInMessage[5]-'A';
			x2=cInMessage[6]-'0';
			result=cInMessage[8]-'0';		//碰子结果
			if(cInMessage[10]>='A' && cInMessage[10]<='L') //对方司令战死后显示军旗位置
				cMap[(cInMessage[10]-'A')][cInMessage[11]-'0']='L';
			switch(result)		//根据不同结果修改棋盘
			{
				case 0:			//对方棋子被己方吃掉
					cMap[y1][x1]='0';
					break;
				case 1:			//对方吃掉己方棋子
					cMap[y2][x2]=cMap[y1][x1];
					cMap[y1][x1]='0';
					break;
				case 2:			//双方棋子对死
					cMap[y1][x1]='0';
					cMap[y2][x2]='0';
					break;
				case 3:			//对方移动棋子
					cMap[y2][x2]=cMap[y1][x1];
					cMap[y1][x1]='0';
					break;
			}
		}
	}
    if(cInMessage[0]=='R')	// RESULT 指令
	{
		y1=cOutMessage[9]-'A';		
		x1=cOutMessage[10]-'0';
		y2=cOutMessage[11]-'A';
		x2=cOutMessage[12]-'0';
		result=cInMessage[7]-'0';		//结果结果
		if(cInMessage[8]==' ' && cInMessage[9]>='A' && cInMessage[9]<='L') //对方司令战死后显示军旗位置
			cMap[(cInMessage[9]-'A')][cInMessage[10]-'0']='L';
		switch(result)		//根据不同结果修改棋盘
		{
			case 0:			//己方棋子被对方吃掉
				cMap[y1][x1]='0';
				break;
			case 1:			//己方吃掉对方棋子
				cMap[y2][x2]=cMap[y1][x1];
				cMap[y1][x1]='0';
				break;
			case 2:			//双方棋子对死
				cMap[y1][x1]='0';
				cMap[y2][x2]='0';
				break;
			case 3:			//己方移动棋子
				cMap[y2][x2]=cMap[y1][x1];
				cMap[y1][x1]='0';
				break;
		}
	}
	
}

/* ************************************************************************ */
/* 函数功能：根据INFO指令,返回参赛队名										*/
/* 接口参数：																*/
/*     char *cInMessage 接收的INFO ver指令									*/
/*     char *cOutMessage 参赛队名											*/
/* ************************************************************************ */
void CulInfo(char *cInMessage,char *cVer,char *cOutMessage)
{
	strcpy(cVer,cInMessage+5);																												//此语句没用
	strcpy(cOutMessage,"NAME 1235678");
}

/* ************************************************************************ */
/* 函数功能：根据START指令,返回布局											*/
/* 接口参数：																*/
/*     char *cInMessage 接收的START first time steps指令					*/
/*     int *iFirst 先行权[0先行，1后行]										*/
/*     int *iTime 行棋时间限制(单位秒)[1000,3600]							*/
/*     int *iStep 进攻等待限制(单位步)[10,31]								*/
/*     char *cOutMessage 布局字符序列										*/
/* ************************************************************************ */
void CulArray(char *cInMessage,int *iFirst,int *iTime,int *iStep,char *cOutMessage)
{
	*iFirst=cInMessage[6]-'0';
	*iTime=cInMessage[8]-'0';
	*iTime=*iTime*10+(cInMessage[9]-'0');
	*iTime=*iTime*10+(cInMessage[10]-'0');
	*iTime=*iTime*10+(cInMessage[11]-'0');
	*iStep=cInMessage[13]-'0';
	*iStep=*iStep*10+(cInMessage[14]-'0');
	if(*iFirst==0)	//先手
		strcpy(cOutMessage,"ARRAY abccddeeffggghhhiiijjkklj");
	else			//后手
		strcpy(cOutMessage,"ARRAY cbacddeeffggghhhiiijjkklj");
}
/* ************************************************************************ */
/* 函数功能：工兵拐弯的搜索                                          		*/
/* 接口参数：																*/
/*     char cTryMap	尝试棋盘局面											*/
/*     int x	横坐标														*/
/*     int y	纵坐标														*/
/*     int iSaMove	记录工兵的铁路线走法									*/
/*     int iNum	已存走法数量												*/
/* 返回值：	初始: iNum=0															*/
/* ************************************************************************ */
int SapperRail(int iSaMove[40][2],char cTryMap[12][5],int x,int y,int iNum)
{
	int i,t;
	//可以前移:不在第一行,不在山界后,前方是空,落在铁道线上
	if (x>0 && !IsAfterHill(x,y) && (cTryMap[x-1][y]=='0' || IsOppChess(cTryMap,x-1,y)) && IsRailway(x-1,y))
	{
		t=0;
		for(i=0;i<=iNum;i++)
		{
			if((x-1)==iSaMove[i][0] && y==iSaMove[i][1])//查出重复走法，跳出循环
				t=1;
		}
		
		if(t==0)
		{
			iNum++;
			iSaMove[iNum][0] = x-1;
			iSaMove[iNum][1] = y;	
			if(cTryMap[x-1][y]=='0')
			{
				cTryMap[x][y]='0';
				cTryMap[x-1][y]='i';
				iNum=SapperRail(iSaMove,cTryMap,x-1,y,iNum);
				cTryMap[x][y]='i';
				cTryMap[x-1][y]='0';
			}
		}		
	}
	
	//可以左移:不在最左列,左侧是空,落在铁道线上
	if (y>0 && (cTryMap[x][y-1]=='0' || IsOppChess(cTryMap,x,y-1)) &&  IsRailway(x,y-1))
	{
		t=0;
		for(i=0;i<=iNum;i++)
		{
			if(x==iSaMove[i][0] && (y-1)==iSaMove[i][1])
				t=1;
		}
		
		if(t==0)
		{
			iNum++;
			iSaMove[iNum][0] = x;
			iSaMove[iNum][1] = y-1;
			if(cTryMap[x][y-1]=='0')
			{
				cTryMap[x][y]='0';
				cTryMap[x][y-1]='i';
				iNum=SapperRail(iSaMove,cTryMap,x,y-1,iNum);
				cTryMap[x][y]='i';
				cTryMap[x][y-1]='0';
			}
		}		
	}
	
	//可以右移://不在最右列,右侧不是己方棋子,落在铁道线上
	if (y<4 && (cTryMap[x][y+1]=='0' || IsOppChess(cTryMap,x,y+1)) && 	IsRailway(x,y+1))
	{
		t=0;
		for(i=0;i<=iNum;i++)
		{
			if(x==iSaMove[i][0] && (y+1)==iSaMove[i][1])
				t=1;
		}
		
		if(t==0)
		{
			iNum++;
			iSaMove[iNum][0] = x;
			iSaMove[iNum][1] = y+1;
			if(cTryMap[x][y+1]=='0')
			{
				cTryMap[x][y]='0';
				cTryMap[x][y+1]='i';
				iNum=SapperRail(iSaMove,cTryMap,x,y+1,iNum);
				cTryMap[x][y]='i';
				cTryMap[x][y+1]='0';
			}
		}
	}
	
	//可以后移:不在最后一行,不在山界前,后方不是己方棋子,落在铁道线上
	if (x<11 && !IsBehindHill(x,y) && (cTryMap[x+1][y]=='0' || IsOppChess(cTryMap,x+1,y)) && 	IsRailway(x+1,y))
	{
		t=0;
		for(i=0;i<=iNum;i++)
		{
			if((x+1)==iSaMove[i][0] && y==iSaMove[i][1])
				t=1;
		}
		
		if(t==0)
		{
			iNum++;
			iSaMove[iNum][0] = (x+1);
			iSaMove[iNum][1] = y;
			if(cTryMap[x+1][y]=='0')
			{
				cTryMap[x][y]='0';
				cTryMap[x+1][y]='i';
				iNum=SapperRail(iSaMove,cTryMap,x+1,y,iNum);
				cTryMap[x][y]='i';
				cTryMap[x+1][y]='0';
			}
		}
	}
	
	return iNum;
}
/*
函数功能： 获取当前行棋的分值
接口参数：
		int i 当前棋子横坐标
		int j 当前棋子纵坐标
		int i1 当前棋子所要走的下一个位置的横坐标
		int j1 当前棋子所要走的下一个位置的纵坐标
		int dz 当前棋盘中对方棋子的概率猜测
		int cMap 当前棋盘
*/
int pinggu(int i,int j,int i1,int j1,int dz[25][14],char cMap[12][5])
{
	int score=0;
	return score;

}

/* ************************************************************************ */
/* 函数功能：计算己方最佳着法(本程序为示例算法,请参赛选手自行改进算法)		*/
/* 接口参数：																*/
/*     char *cMap 棋盘局面													*/
/*     char *cInMessage 来自裁判的 GO 命令									*/
/*     char *cOutMessage 发给裁判的 BESTMOVE 命令							*/
/* ************************************************************************ */

void CulBestmove(char cMap[12][5],char *cInMessage,char *cOutMessage,int dz[25][14])																	
{	
	strcpy(cOutMessage,"BESTMOVE A0A0");
	int move[4];
	int allmove[200][4],iSaMove[40][2];
	int i,j,num,num1,i1,j1,k=0;
	char cmap[12][5];
	int score1,score=-10000;

	FILE *fp;
	
   	for(i=0;i<12;i++)
		for(j=0;j<5;j++)
		{
			if(IsMyMovingChess(cMap,i,j) && !IsBaseCamp(i,j))  //己方不在大本营的可移动棋子
			{
				//工兵形棋
				if(cMap[i][j]=='i' && IsRailway(i,j))
				{
					iSaMove[0][0]=i;
					iSaMove[0][1]=j;
					for(i1=0;i1<12;i1++)
					{
						for(j1=0;j1<5;j1++)
							cmap[i1][j1]=cMap[i1][j1];
					}
					num=SapperRail(iSaMove,cmap,i,j,0);
					for(num1=1;num1<num;num1++)
					{
						allmove[k][0]=i;
						allmove[k][1]=j;
						allmove[k][2]=iSaMove[num1][0];
						allmove[k][3]=iSaMove[num1][1];	
						k++;
					}
				}
				//可以前移:不在第一行,不在山界后,前方不是己方棋子,前方不是有棋子占领的行营
		 		if(i>0 && !IsAfterHill(i,j) && !IsMyChess(cMap,i-1,j) && !IsFilledCamp(cMap,i-1,j))
				{
					allmove[k][0]=i;
					allmove[k][1]=j;
					allmove[k][2]=i-1;
					allmove[k][3]=j;
					k++;
					if((j==0 || j==4) && i>0 && i<11)
					{
						for(i1=i-1;i1>0;i1--)
						{
							if(cMap[i1][j]=='0')
							{
								allmove[k][0]=i;
								allmove[k][1]=j;
								allmove[k][2]=i1;
								allmove[k][3]=j;
								k++;
							}
							else if(cMap[i1][j]>='A' && cMap[i1][j]=='X')
							{
								allmove[k][0]=i;
								allmove[k][1]=j;
								allmove[k][2]=i1;
								allmove[k][3]=j;
								k++;
								break;
							}
							else break;
						}
					}
				}		
				//可以左移:不在最左列,左侧不是己方棋子,左侧不是被占用的行营
				if(j>0 && !IsMyChess(cMap,i,j-1) && !IsFilledCamp(cMap,i,j-1))
				{
					allmove[k][0]=i;
					allmove[k][1]=j;
					allmove[k][2]=i;
					allmove[k][3]=j-1;
					k++;
					//是否在1,5,6,10铁道上
					if(i==1||i==5||i==6||i==10)
					{
						for(int i1=j-1;i1>=0;i1--)
						{
							if(cMap[i][i1]=='0')
							{
								allmove[k][0]=i;
								allmove[k][1]=j;
								allmove[k][2]=i;
								allmove[k][3]=i1;
								k++;
							}
							else if(cMap[i][i1]>='A'&&cMap[i][i1]<='X')
							{
								allmove[k][0]=i;
								allmove[k][1]=j;
								allmove[k][2]=i;
								allmove[k][3]=i1;
								k++;
								break;
							}
							else break;
						}
					}
				}
					//可以右移:不在最右列,右侧不是己方棋子,右侧不是被占用的行营
				if(j<4 && !IsMyChess(cMap,i,j+1) && !IsFilledCamp(cMap,i,j+1))
				{
					allmove[k][0]=i;
					allmove[k][1]=j;
					allmove[k][2]=i;
					allmove[k][3]=j+1;
					k++;
					//是否在1,5,6,10铁道上
					if(i==1||i==5||i==6||i==10)
					{
						for(int i1=j+1;i1<5;i1++)
						{
							if(cMap[i][i1]=='0')
							{
								allmove[k][0]=i;
								allmove[k][1]=j;
								allmove[k][2]=i;
								allmove[k][3]=i1;
								k++;
							}
							else if(cMap[i][i1]>='A'&&cMap[i][i1]<='X')
							{
								allmove[k][0]=i;
								allmove[k][1]=j;
								allmove[k][2]=i;
								allmove[k][3]=i1;
								k++;
								break;
							}
							else break;
						}
					}
				}
					  
	     		//可以后移：不在最后一列，后方不是己方旗子，后面不是已占有的行营
				if(i<11 && !IsMyChess(cMap,i+1,j) && !IsFilledCamp(cMap,i+1,j))
				{	
					allmove[k][0]=i;
					allmove[k][1]=j;
					allmove[k][2]=i+1;
					allmove[k][3]=j;
					k++;
					//可以在铁轨两侧向后移动，后方不是己方旗子，不是第一行，不在最后一行
					if((j==0 || j==4) && i>0 && i<11)
					{
						for(i1=i+1;i1<11;i1++)
						{
							if(cMap[i1][j]=='0')
							{
								allmove[k][0]=i;
								allmove[k][1]=j;
								allmove[k][2]=i1;
								allmove[k][3]=j;
								k++;
							}
							else if(cMap[i1][j]>='A' && cMap[i1][j]<='X')
							{
								allmove[k][0]=i;
								allmove[k][1]=j;
								allmove[k][2]=i1;
								allmove[k][3]=j;
								k++;
								break;
							}
							else  break;
						}

					}
				}
				//可以左前移：左前方不是已占有的行营，不是己方旗子
				if((Iszuoqian(i,j) || IsMoveCamp(i,j)) && !IsMyChess(cMap,i-1,j-1) && !IsFilledCamp(cMap,i-1,j-1))
				{
					allmove[k][0]=i;
					allmove[k][1]=j;
					allmove[k][2]=i-1;
					allmove[k][3]=j-1;
					k++;
				}
				//可以左后移：左后方不是己方旗子，不是已占有的行营
		    	if((Iszuohou(i,j) || IsMoveCamp(i,j)) && !IsMyChess(cMap,i+1,j-1) && !IsFilledCamp(cMap,i+1,j-1))
				{
					allmove[k][0]=i;
					allmove[k][1]=j;
					allmove[k][2]=i+1;
					allmove[k][3]=j-1;
					k++;
				}
				//可以右前移：右前方不是己方旗子，不是已占有的行营
		    	if((Isyouqian(i,j) || IsMoveCamp(i,j)) && !IsMyChess(cMap,i-1,j+1) && !IsFilledCamp(cMap,i-1,j+1))
				{
					allmove[k][0]=i;
					allmove[k][1]=j;
					allmove[k][2]=i-1;
					allmove[k][3]=j+1;
					k++;
				}
				//可以右后移：右后方不是己方旗子，不是已占有的行营
			   if((Isyouhou(i,j) || IsMoveCamp(i,j)) && !IsMyChess(cMap,i+1,j+1) && !IsFilledCamp(cMap,i+1,j+1))
				{
					allmove[k][0]=i;
					allmove[k][1]=j;
					allmove[k][2]=i+1;
					allmove[k][3]=j+1;
					k++;
				}
			}	
		}	
		for(i=0;i<k;i++)
		{
			if((score1=pinggu(allmove[i][0],allmove[i][1],allmove[i][2],allmove[i][3],dz,cMap))>=score)
			{			
				if(allmove[i][0]==allmove[i][2] && allmove[i][1]==allmove[i][3]) 
					continue;    
				else		
				{
					score=score1;
					move[0]=allmove[i][0];
					move[1]=allmove[i][1];
					move[2]=allmove[i][2];
					move[3]=allmove[i][3];
				}
			}
		}
		cOutMessage[9]=move[0]+'A';
     	cOutMessage[10]=move[1]+'0';
	    cOutMessage[11]=move[2]+'A';
     	cOutMessage[12]=move[3]+'0';
		fp=fopen("1","a");
		fprintf(fp,"\n%d,%d,%d,%d,%d,%d\n",move[0],move[1],move[2],move[3],score1,score);
	//	fprintf(fp,"\n%d\n",zz++);
		fclose(fp);
		
}

int main()
{
	char cVer[200];			//协议版本
	int iFirst;				//先行权[0先行，1后行]																							//不知道iFirst、iTime、iStep有什么用？可能与平台有关系。
	int iTime;				//行棋时间限制(单位秒)[1000,3600]
	int iStep;				//进攻等待限制(单位步)[10,31]
	char cInMessage[200];   //输入通信内容
	char cOutMessage[200];  //输出通信内容
    char cMap[12][5];		//棋盘
       //   司令 ,军长 ,师长 ,旅长 ,团长 ,营长 ,连长 ,排长 ,工兵 ,地雷 ,炸弹 ,军旗     i,j,(该子所在位置)
    int dz[25][14]={  
		    100  ,100  ,250  ,500  ,500  ,700  ,2000 ,2100 ,400  ,3250 ,100  ,0    ,   0,0,//A0  10000
			0    ,0    ,0    ,0    ,0    ,0    ,0    ,3000 ,0    ,2000 ,0    ,5000 ,   0,1,//A1
			100  ,100  ,250  ,500  ,500  ,700  ,2000 ,2100 ,200  ,3250 ,100  ,0    ,   0,2,//A2
			0    ,0    ,0    ,0    ,0    ,0    ,0    ,3000 ,0    ,2000 ,0    ,5000 ,   0,3,//A3
			100  ,100  ,250  ,500  ,500  ,700  ,2000 ,2100 ,400  ,3250 ,100  ,0    ,   0,4,//A4
			
			500  ,500  ,850  ,700  ,600  ,500  ,600  ,500  ,1000 ,3250 ,1000 ,0    ,   1,0,//B0
			400  ,400  ,850  ,700  ,600  ,500  ,800  ,600  ,1000 ,3250 ,900  ,0    ,   1,1,//B1
			400  ,400  ,850  ,700  ,600  ,500  ,800  ,600  ,1000 ,3250 ,900  ,0    ,   1,2,//B2
			400  ,400  ,850  ,700  ,600  ,500  ,800  ,600  ,1000 ,3250 ,900  ,0    ,   1,3,//B3
			500  ,500  ,850  ,700  ,600  ,500  ,600  ,500  ,1000 ,3250 ,1000 ,0    ,   1,4,//B4
			
			500  ,500  ,1000 ,1000 ,1000 ,1500 ,500  ,500  ,2000 ,0    ,1500 ,0    ,   2,0,//C0
			500  ,500  ,1000 ,1000 ,1000 ,1500 ,500  ,500  ,2000 ,0    ,1500 ,0    ,   2,2,//C2
			500  ,500  ,1000 ,1000 ,1000 ,1500 ,500  ,500  ,2000 ,0    ,1500 ,0    ,   2,4,//C4
			
			500  ,500  ,1000 ,1000 ,1000 ,1500 ,500  ,500  ,2000 ,0    ,1500 ,0    ,   3,0,//D0
			500  ,500  ,1000 ,1000 ,1000 ,1500 ,500  ,500  ,2000 ,0    ,1500 ,0    ,   3,1,//D1
			500  ,500  ,1000 ,1000 ,1000 ,1500 ,500  ,500  ,2000 ,0    ,1500 ,0    ,   3,3,//D3
			500  ,500  ,1000 ,1000 ,1000 ,1500 ,500  ,500  ,2000 ,0    ,1500 ,0    ,   3,4,//D4
			
			500  ,500  ,1000 ,1000 ,1000 ,1500 ,500  ,500  ,2000 ,0    ,1500 ,0    ,   4,0,//E0
			500  ,500  ,1000 ,1000 ,1000 ,1500 ,500  ,500  ,2000 ,0    ,1500 ,0    ,   4,2,//E2
			500  ,500  ,1000 ,1000 ,1000 ,1500 ,500  ,500  ,2000 ,0    ,1500 ,0    ,   4,4,//E4
			
			500  ,500  ,1000 ,1000 ,1200 ,2800 ,1500 ,1500 ,0    ,0    ,0    ,0    ,   5,0,//F0
			500  ,500  ,1000 ,1000 ,1000 ,1000 ,1500 ,1500 ,2000 ,0    ,0    ,0    ,   5,1,//F1
			500  ,500  ,1000 ,1000 ,1100 ,2800 ,1600 ,1500 ,0    ,0    ,0    ,0    ,   5,2,//F2
			500  ,500  ,1000 ,1000 ,1000 ,1000 ,1500 ,1500 ,2000 ,0    ,0    ,0    ,   5,3,//F3
			500  ,500  ,1000 ,1000 ,1200 ,2800 ,1500 ,1500 ,0    ,0    ,0    ,0    ,   5,4,//F4
	};   //猜测概率                  10000     22200  27600
	
	cin.getline(cInMessage,200);		//获取来自裁判系统的指令 "GO 0000 0 00"
	while(cInMessage[0]>='A')
	{
		switch(cInMessage[0])
		{
			case 'I':								//INFO指令
				CulInfo(cInMessage,cVer,cOutMessage);
				cout<<cOutMessage<<endl;			//将"NAME "指令传递给裁判系统
				break;
			case 'S':								//START 指令 
				CulArray(cInMessage,&iFirst,&iTime,&iStep,cOutMessage);	
				InitMap(cMap,cOutMessage);
				cout<<cOutMessage<<endl;
				break;
			case 'G':								//GO 指令
				FreshMap(cMap,cInMessage,cOutMessage);
				CulBestmove(cMap,cInMessage,cOutMessage,dz);
				cout<<cOutMessage<<endl;
				break;
			case 'R':								//RESULT 指令
				FreshMap(cMap,cInMessage,cOutMessage);
				break;
			case 'E':								//END 指令
				return 0;	
			default:
				return 1;				
		}
		cin.getline(cInMessage,200);	//获取来自裁判系统的指令"START" 或 "RESULT" 或 "GO" 或 "END"
	}
	return 0;
}
