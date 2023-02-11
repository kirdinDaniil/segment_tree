# segment_tree

Дерево отрезков — это структура данных, которая позволяет алгоритмически просто и логарифмически быстро находить сумму элементов массива на заданном отрезке. 
Дерево отрезков выполняется на массиве и нумерация обычно начинается с единицы, размером в четыре раза большем чем изначальное количество чисел, так как дерево отрезков является полным двоичным деревом (максимальное возможное количество ребер) поэтому нам нужно вмещать как можно больше элементов

        int tree[4*(n+1)];

## Построение дерева
Процесс построения дерева заключается в заполнении массива t. Заполним этот массив таким образом, чтобы i-й элемент являлся бы результатом некоторой бинарной операции (для каждой конкретной задачи своей) от элементов c номерами 2i+1 и 2i+2, то есть родитель являлся результатом бинарной операции от своих сыновей (обозначим в коде эту операцию как "∘"). Один из вариантов — делать рекурсивно. Пусть у нас имеются исходный массив a, а также переменные tl и tr, обозначающие границы текущего полуинтервала. Запускаем процедуру построения от корня дерева отрезков (i=0, tl=0, tr=n), а сама процедура построения, если её вызвали не от листа, вызывает себя от каждого из двух сыновей и суммирует вычисленные значения, а если её вызвали от листа — то просто записывает в себя значение этого элемента массива (Для этого у нас есть исходный массив a). Асимптотика построения дерева отрезков составит, таким образом, O(n). 

        void buildTree(int mas[],int tree[],int index,int leftRange,int rightRange){
            if (leftRange == rightRange)
                tree[index] = mas[leftRange];
            else{
                int middleRange = (leftRange + rightRange)/2;
                buildTree(mas,tree,index*2,leftRange,middleRange);
                buildTree(mas,tree,index*2+1,middleRange+1,rightRange);
                tree[index] = tree[index*2] + tree[index*2 + 1];
            }
        }
        

## Сумирование дерева

      int sumTree(int tree[],int leftRequest,int rightRequest,int index,int leftRange,int rightRange){
          if(leftRequest <= leftRange && rightRange <= rightRequest)
              return tree[index];
          if(rightRange < leftRequest || rightRequest < leftRange){
              return 0;
          }
          int middleRange = (leftRange + rightRange)/2;
          return sumTree(tree,leftRequest,rightRequest,index*2,leftRange,middleRange) +
                  sumTree(tree,leftRequest,rightRequest,index*2+1,middleRange+1,rightRange);
      }
      
  ## Обновление дерева
  
        void updateTree(int mas[],int tree[], int updateIndex,int value, int index,int leftRange,int rightRange){
            if(updateIndex <= leftRange && rightRange <= updateIndex){
                mas[updateIndex] = value;
                tree[index] = value;
                return;
            }
            if(leftRange > updateIndex || updateIndex > rightRange){
                return;
            }
            int middleRange = (leftRange + rightRange)/2;
            updateTree(mas,tree,updateIndex,value,index*2,leftRange,middleRange);
            updateTree(mas,tree,updateIndex,value,index*2+1,middleRange+1,rightRange);
            tree[index] = tree[index*2] + tree[index*2 + 1];
        }
