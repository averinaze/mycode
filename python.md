# mycode
nospace = "X-DSPAM-Confidence:    0.8475";
findzero=nospace.find('0')
find5=nospace.find('5')
result=nospace[findzero:find5+1]
print(float(result))

# Use the file name mbox-short.txt as the file name
fname = input("Enter file name: ")
fh = open(fname)
count=0
var=0

for line in fh:
    if line.startswith('X-DSPAM-Confidence:'):
        data=line.find(':')
        var2=float(line[data+1:])
        var=var+var2
        count=count+1
            
print('Average spam confidence:',var/count)

fname=input('Enter file name:')
handle=open(fname)
dictionary=dict()
for line in handle:
    if line.startswith('From '):
        baris=line.split()
        x=baris[1]
        dictionary[x]=dictionary.get(x,0)+1

bigsender=0
bigcount=0
for i in dictionary:
    if dictionary[i]>bigcount:
        bigcount=dictionary[i]
        bigsender=i
print(bigsender,bigcount)

fname=input('Enter file name:')
handle=open(fname)
a=dict()

for line in handle:
    if line.startswith('From '):
        lst=line.split()
        lst_hrs=lst[5].split(':')
        hr=lst_hrs[0]
        a[hr]=a.get(hr,0)+1

        
b=list()
for x,y in a.items():
    newlist=(x,y)
    b.append(newlist)
    
b=sorted(b)
for x,y in b:
    print(x,y)
    
 import re
numlist=list()
for line in open('/Users/HP/test1.txt','r'):
    y=re.findall('[0-9]+',line)
    for i in y:
        z=int(i)
        numlist.append(z)
print(sum(numlist))
