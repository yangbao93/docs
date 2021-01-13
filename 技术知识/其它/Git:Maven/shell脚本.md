# shell脚本
通过换行可以避免书写（；），但如果将代码写作一行则需要书写（；）

echo 输出

双引号（”）：
单引号（’）：
反点（ ` ）：

read

expr 和 let

if 控制：
if condition1
then
//do anything
elif condition2
then
//do anything
else
//do anything
fi

或者
if [ $(ps -ef | grep -c “ssh") -gt 1 ]; then echo “true”; fi

for控制：
for var in item1 item2 … itemN
do
//do anything

done

或者
for var in item1 item2 … itemN; do echo ${var}; echo “continue…”; done;

where控制：
while condition
do
//

done

举例：
int=1
while(( $int<=5 ))
do
echo $int

let “int++”
done

until控制（直到条件后停止）：
until condition
do
//
done

举例：
a=0
until [ ! $a -lt 10 ]
do
echo $a
a= ` expr $a +1 `
done

case控制：
case 值 in
模式1)
command1
command2

command3

;;

模式2)
command1
command2

;;
esac

举例：
echo “输入1-3之间的数字：”
ehco ‘你输入的数字为：’
read aNum
case $aNum in
1)

echo ‘你选择了1’

;;
2)
echo ‘你选择了2’

;;

3) echo ’你选择了3’
;;

*) echo ‘输入的数字超过范围’
;;

esac

break控制：

```

```