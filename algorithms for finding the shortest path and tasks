import math
from os import path
import networkx as nx
from operator import itemgetter
import pandas as pd
from pandas import ExcelWriter
import time
from datetime import datetime

result =[]

def arg_min(s_ways, use_vart):
    amin = -1
    m = math.inf  
    for i, t in enumerate(s_ways):
        if t < m and i not in use_vart:
            m = t
            amin = i

    return amin

def Diikstra(matr, start, end, D_time):
    N = len(matr)  
    s_ways = [math.inf]*N  

    v = start
    use_vart = {v}  
    s_ways[v] = 0   
    optimal = [0]*N   


    while v != -1:          
        for j, dw in enumerate(matr[v]):   
            if j not in use_vart:          
                w = s_ways[v] + dw
                if w < s_ways[j]:
                    s_ways[j] = w
                    optimal[j] = v       

        v = arg_min(s_ways, use_vart)        
        if v >= 0:                    
            use_vart.add(v)               

    print('Кратчайший путь из начальной вершины')
    print('до вершины |  вес')
    for i in range(0,N):
        print('     {}     |   {}'.format(i+1, s_ways[i]))
    #print(s_ways, optimal, sep="\n")
    P = [end]
    while end != start:
        end = optimal[P[-1]]
        P.append(end)
    
    ans=0
    for i in range(0, len(P)-1):
        ans+=matr[P[i+1]][P[i]]    
    print(ans)
    way = [x+1 for x in P]
    print('Кратчайший путь: {} имеет вес {}'.format(way, ans))
    result.append({
        'Start_v': str(start+1),
        'End_v': str(end+1),
        'D_answer': str(ans),
        'D_short_way': str(way),
        'Time': str(datetime.now() - D_time)
    })
    
def get_path(P, u, v):
    path = [u]
    while u != v:
        u = P[u][v]
        path.append(u)

    return path

def Floid(matr, start, end, F_time):
    N = len(matr)                       
    P = [[v for v in range(N)] for u in range(N)]     

    for k in range(N):
        for i in range(N):
            for j in range(N):
                d = matr[i][k] + matr[k][j]
                if matr[i][j] > d:
                    matr[i][j] = d
                    P[i][j] = k    
    #print(P[start])
    way1 = get_path(P,end, start)
    way = [x+1 for x in way1]
    # print(way1)
    ans=0
    for i in range(0, len(way1)-1):
        ans += matr[way1[i+1]][way1[i]]
    #print(ans)
    if ans != math.inf:
        print('Кратчайший путь: {} имеет вес {}'.format(way, ans))
    else: 
        print('Пути нет')
    result.append({
        'Start_v': str(start+1),
        'End_v': str(end+1),
        'F_answer': str(ans),
        'Time': str(datetime.now() - F_time)
    })
        

def min_el(lst,myindex):
    return min(x for idx, x in enumerate(lst) if idx != myindex)

def Delete(matr,i1,i2):
    del matr[i1]
    for i in matr:
        del i[i2]
    return matr


def Komivoyazher(matr):
    n=len(matr)
    way=0
    i_str=[]
    i_row=[]
    res=[]
    answer=[]
    oldmatr=[]
    
    for i in range(n):
        i_str.append(i)
        i_row.append(i)

    for i in range(n):oldmatr.append(matr[i].copy())
    for i in range(n): matr[i][i]=math.inf
    H=0
    while True:
        for i in range(len(matr)):
            temp=min(matr[i])
            H+=temp
            for j in range(len(matr)):
                matr[i][j]-=temp   
        for i in range(len(matr)):
            temp = min(row[i] for row in matr)
            H+=temp
            for j in range(len(matr)):
                matr[j][i]-=temp
        mp=0
        i1=0
        i2=0
        tmp=0
        for i in range(len(matr)):
            for j in range(len(matr)):
                if matr[i][j]==0:
                    tmp=min_el(matr[i],j)+min_el((row[j] for row in matr),i)
                    if tmp>=mp:
                        mp=tmp
                        i1=i
                        i2=j

        res.append(i_str[i1]+1)
        res.append(i_row[i2]+1)
        oldi1=i_str[i1]
        oldi2=i_row[i2]
        if oldi2 in i_str and oldi1 in i_row:
            Newi1=i_str.index(oldi2)
            Newi2=i_row.index(oldi1)
            matr[Newi1][Newi2]=float('inf')
        del i_str[i1]
        del i_row[i2]
        matr=Delete(matr,i1,i2)
        if len(matr)==1:break
        
    for i in range(0,len(res)-1,2):
        if res.count(res[i])<2:
            answer.append(res[i])
            answer.append(res[i+1])
    for i in range(0,len(res)-1,2):
        for j in range(0,len(res)-1,2):
            if answer[len(answer)-1]==res[j]:
                #answer.append(res[j])
                answer.append(res[j+1])
    
    for i in range(0,len(answer)-1,2):
        if i==len(answer)-2:
            way+=oldmatr[answer[i]-1][answer[i+1]-1]
            way+=oldmatr[answer[i+1]-1][answer[0]-1]
        else: way+=oldmatr[answer[i]-1][answer[i+1]-1]
    # print(answer)    
    # print(way)
    print('Путь комивояжера: {} имеет вес {}'.format(answer, way))
    # if len(way) == len(matr):
    #     print('Эйлеров цикл равен: {}'.format(way))

def get_edge_and_index(v, graph):
    edge = ()
    index = -1

    for i in range(len(graph)):
        if v == graph[i][0] or v == graph[i][1]:
            edge, index = graph[i], i
            break

    return index, edge

def find_eulerian_tour(matr):
    graph = []
    n = len(matr[0])

    for i in range(n):
        for j in range(i, n):
            if matr[i][j] > 0:
                graph.append([i, j])

    stack = []
    tour = []

    stack.append(graph[0][0])  

    v = stack[len(stack) - 1]
    vertex = [item for sublist in graph for item in sublist]
    degree = vertex.count(v)

    if degree % 2 != 0:  
        print('Эйлерова цикла нет')
        return

    while len(stack) > 0:
        v = stack[len(stack) - 1] 

        vertex = [item for sublist in graph for item in sublist]
        degree = vertex.count(v)

        if degree == 0:
            stack.pop()
            tour.append(v)
        else:
            index, edge = get_edge_and_index(v, graph)
            graph.pop(index)
            stack.append(edge[1] if v == edge[0] else edge[0])

    for i in range(len(tour)):
        tour[i] += 1
        
    if tour is not None:
        print(*tour, sep=' - ')
    return tour[::-1]



def coloring(matr):
    num = len(matr)
    matrx_copy = [[0]*num for i in range(num)]
    for i in range(num):
        for j in range(num):
            if matr[i][j] == 0:
                matrx_copy[i][j] = 0
            else:
                matrx_copy[i][j] = 1
    colored=[[]]
    color=0
    colored_list=[]
    num2=num1=-1

    def loop(num):
        for t in colored[color]:
            if matrx_copy[t][num]==1:
                return False
        return True
    
    for a in matrx_copy:
        num1+=1
        if num1 not in colored_list:
            if not loop(num1):
                colored.append([])
                color+=1
                colored_list.append(num1)
                colored[color].append(num1)
            num2=num1
            for b in a[num1:]:
                if b==0 and num2 not in colored_list and loop(num2):
                    colored_list.append(num2)
                    colored[color].append(num2)  
                num2+=1
                

    for i in range(len(colored)):
        for j in range(len(colored[i])):
            colored[i][j]+=1
            
    colored_res = ''
    for i in range(len(colored)):
        colored_res +=f'{i+1}: {str(colored[i])}; '
    return colored_res


ways=[]
    
def eiler(data, row, i, ways):
    data [row] [i] = 3

    if row+1<len(data) and data[row+1][i]==1:
        if not eiler(data,row+1,i,ways):
            data[row+1][i]=1

    if row-1>-1 and data[row-1][i]==1:
        if not eiler(data,row-1,i,ways):
            data[row-1][i]=1

    if i+1< len(data[0])and data[row][i+1]==1:
        if not eiler(data,row,i+1,ways):
            data[row][i+1]=1
     
    if i-1 >-1 and data[row][i-1]==1:
        if not eiler(data,row,i-1,ways):
            data[row][i-1]=1
    
    
    for row1 in data:
        for i1 in row1:
            if(i1==1):
                return False
    ways.append((row+1,i+1))
    return True


def save_exel():
    dataframe = pd.DataFrame(result)
    writer = ExcelWriter(f'result.xlsx')
    dataframe.to_excel(writer, 'result')
    writer.save()
    print(f'Данные сохранены в файл "result.xlsx"')


def read_f(Path):
    with open(Path) as f:
        matrix = [list(map(int, row.split())) for row in f.readlines()]    
    for i in matrix:
        print(*i)  
    edges_fp =0
    N = len(matrix)
    for i in range(0, N):
        for j in range(0, N):
            if matrix[i][j] == 0 and i!= j:
                matrix[i][j] = math.inf
            elif matrix[i][j] < 0:
                matrix[i][j] = matrix[i][j]*(-1)
                edges_fp += 1
            else:
                edges_fp +=1
    result.append({
        'Vartex': str(N),
        'Edges': str(edges_fp),
    })    
    return matrix  

if __name__ == '__main__':
    menu = "6"
    while menu != "0":
        print('\n1 - Алгоритм Дийкстры \n2 - Алгоритм Флойда\n3 - Коммивояжер\n4 - Раскраска\n5 - Эйлеров цикл\n6 - Сохранить результат\n0 - Выход\n')    
        menu = input()

        match menu:
            case "1":
                Path = 'matrix.txt'
                matr = read_f(Path)
                N = len(matr)
                n = list(map(int,input('Введите начальную и конечную вершину от 1 до {} : '.format(N)).split()))
                if n[0] <1 or n[1]>N:
                    print('Введённое значение некорректно')
                else:
                    print()
                    D_time = datetime.now()
                    Diikstra(matr, n[0]-1, n[1]-1, D_time)
            case "2":
                Path = 'for_way.txt'
                print()
                matr = read_f(Path)
                N = len(matr)
                x, y = list(map(int,input('Введите начальную и конечную вершину от 1 до {} : '.format(N)).split()))
                if x <1 or y>N:
                    print('Введённое значение некорректно')
                else:
                    F_time = datetime.now()
                    Floid(matr, x-1, y-1, F_time)
            case "3":
                Path = 'matrix.txt'
                matr = read_f(Path) 
                Komivoyazher(matr)     
            case "4":
                print('Задача с раскраской вершин: ')
                Path ='color.txt'
                with open(Path) as f:
                    matr = [list(map(int, row.split())) for row in f.readlines()]
                for i in matr:
                    print(*i)
                
                print(coloring(matr))
            case "5":
                Path = 'eil.txt'
                with open(Path) as f:
                    matr = [list(map(int, row.split())) for row in f.readlines()]
                find_eulerian_tour(matr)

            case "6":
                save_exel()
            case "0":
                break
            case "8":
                Path = 'eil.txt'
                matr = read_f(Path)
                N=len(matr)
                n = list(map(int,input('Введите начальную и конечную вершину от 1 до {} : '.format(N)).split()))
                Dv_time = datetime.now()
                matr1 = [[v for v in range(N)] for u in range(N)] 
                for i in range(0, N):
                    for j in range(0, N):
                        if matr[i][j] == 0 or matr[i][j] == math.inf:
                            matr1[i][j] = math.inf 
                        else:
                            matr1[i][j] = 1
                Diikstra(matr1, n[0]-1, n[1]-1, Dv_time)
            case _:
                print('Недопустимое значение')


    
