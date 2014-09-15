/**************************************************************************************************/
/* Copyright (C) hanqiong.com, USTC, 2014-2015                                                    */
/*                                                                                                */
/*  FILE NAME             :  menu.c                                                               */
/*  PRINCIPAL AUTHOR      :  Hanqiong                                                             */
/*  SUBSYSTEM NAME        :  menu                                                                 */
/*  MODULE NAME           :  menu                                                                 */
/*  LANGUAGE              :  C                                                                    */
/*  TARGET ENVIRONMENT    :  ANY                                                                  */
/*  DATE OF FIRST RELEASE :  2014/09/14                                                           */
/*  DESCRIPTION           :  This is a menu program                                               */
/**************************************************************************************************/

/*
 * Revision log :
 *
 * Created by Hanqiong, 2014/09/14
 *
*/

#include<stdio.h>
#include<stdlib.h>

#define CMD_MAX_LEN 128
#define DESC_LEN    1024
#define CMD_NUM     10

/* data struct and its operation */

int Help();

typedef struct DataNode
{
    char*   cmd;
    char*   desc;
    int     (*handler)();
    struct   DataNode *next;
}tDataNode;

static tDataNode head[] =
{
    {"help", "this is help cmd", Help, &head[1]},
    {"version", "menu program v1.0", NULL, &head[2]},
    {"pwd", "this is pwd cmd", NULL, &head[3]},
    {"mkdir", "this is mkdir cmd", NULL, &head[4]},
    {"rmdir", "this is rmdir cmd", NULL, &head[5]},
    {"cd", "this is cd cmd", NULL, &head[6]},
    {"chmod", "this is chmod cmd", NULL, NULL}
};
void main()
{
    tDataNode *p = head;
    /* cmd line begins */
    while(1)
    {
        char cmd[CMD_MAX_LEN];
        printf("Input a cmd number > ");
        scanf("%s", cmd);
        p=head;
        while(p != NULL)
        {
            if(!strcmp(p->cmd, cmd))
            {
                printf("%s - %s\n", p->cmd, p->desc);
                if(p->handler != NULL)
                {
                    p->handler();
                }
                    break;
            }
            p=p->next;
        }
        if(p== NULL)
        {
            printf("This is a wrong cmd!\n ");
        }
      }
}
int Help()
{
    printf("Menu List:\n");
    tDataNode * p = head;
    while(p != NULL)
    {
        printf("%s - %s\n", p->cmd, p->desc);
        p=p->next;
    }
    return 0;
}
