# hse22_hw1
Создам ссылки файлов в моей папке
```
ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq  
ln -s /usr/share/data-minor-bioinf/assembly/oil_R2.fastq  
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq  
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq  
```
Отберу случайные чтения
```
seqtk sample -s606 /usr/share/data-minor-bioinf/assembly/oil_R1.fastq 5000000 > R1_paired_end.fastq  
seqtk sample -s606 /usr/share/data-minor-bioinf/assembly/oil_R2.fastq 5000000 > R2_paired_end.fastq  
seqtk sample -s606 /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq 1500000 > R1_mate_end.fastq   
seqtk sample -s606 /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq 1500000 > R2_mate_end.fastq   
```
С помощью программ fastQC и multiQC оценю качество исходных чтений
```
mkdir fastqc      
ls *_end.fastq | xargs -P 4 -tI{} fastqc -o fastqc {}  
mkdir multiqc      
multiqc -o multiqc fastqc
```
Отчет
![image](https://user-images.githubusercontent.com/114064027/193250715-c84dff96-19e6-40a5-b088-6195f6ed021e.png)
![image](https://user-images.githubusercontent.com/114064027/193250894-e50a027c-d528-4e9f-abe6-6a5c06d715ea.png)
![image](https://user-images.githubusercontent.com/114064027/193250981-226eb64f-2f73-4b3a-8485-a948d05ffa22.png)

Подрежу полученные чтения
```
platanus_trim R1_paired_end.fastq R2_paired_end.fastq
platanus_internal_trim R1_mate_end.fastq R2_mate_end.fastq
```
Снова проверю качество с помощью программ fastQC и multiQC:
```
mkdir Trim
ls *trimmed | xargs -tI{} fastqc -o Trim {}
mkdir multiqc2  
multiqc -o multiqc2 Trim 
```
Данные из отчёта
![image](https://user-images.githubusercontent.com/114064027/193251244-87bf7d02-a5b7-4731-bab7-684592eb2e8f.png)
![image](https://user-images.githubusercontent.com/114064027/193251301-f776798d-c723-4077-8e2e-c6cd52f8c7fc.png)
![image](https://user-images.githubusercontent.com/114064027/193251363-8c1997f7-ea59-407d-9dbc-3cee7a6ab3d5.png)
После подрезания уменьшилось количество чтений (M Seqs), средняя длина (Length). Согласно графику Sequence Quality Histograms, улучшилось качество.

С помощью программы “platanus assemble” собреру контиги из подрезанных чтений
```
mkdir platanus
time platanus assemble -o Poil -t 8 -n 20 -f R1_paired_end.fastq.trimmed R2_paired_end.fastq.trimmed 2> ~/platanus/Final1.log
```
С помощью программы “ platanus scaffold” соберу скаффолды из контигов, а также из подрезанных чтений

```
time platanus scaffold -o Poil -t 1 -c Poil_contig.fa -IP1 *.trimmed -OP2 *.int_trimmed 2> ~/platanus/Final_scaffold.log
```
С помощью программы “ platanus gap_close” уменьшу кол-во гэпов с помощью подрезанных чтений
```
time platanus gap_close -o Poil -t 1 -c Poil_scaffold.fa -IP1 *.trimmed -OP2 *.int_trimmed 2> gapclose.log
```
Проанализирую полученные с помощью platanus данные в Google Collab
https://colab.research.google.com/drive/1Swqf955lHseiRPRSuiOpiZt90z_LUfM6?usp=sharing

![image](https://user-images.githubusercontent.com/114064027/193255753-4b876332-d638-4170-ade5-d12bfcbe579d.png)

![image](https://user-images.githubusercontent.com/114064027/193258598-c8d6a510-4bcd-4c82-b6c1-23786553c13a.png)

![image](https://user-images.githubusercontent.com/114064027/193260636-6f7e1948-2f3c-4764-8ed5-e92f7570a026.png)
**Общее количество контигов = 622**

**Общая длина = 3927197**

**Длина самого длинного контига = 175819**

**N50 = 55863**

Анализ скаффолдов

![image](https://user-images.githubusercontent.com/114064027/193263004-e34bed34-6a8a-41ca-ad9c-d318ead933be.png)

Так как самый длинный скаффолд покрывает 50%, то N50=длине самого длинного скаффолда

**Общее количество скаффолдов = 67**

**Общая длина = 3874886**

**Длина самого длинного скаффолда = 3839079**

**N50 = 3839079**

Для самого длинного скаффолда посчитаю количество гэпов

![image](https://user-images.githubusercontent.com/114064027/193797831-80af61cc-b554-4e15-be85-ee86a7d52ebd.png)

**Количество участков N = 155

Количество N = 7524**

Проанализирую данные, полученные после работы platanus gap_close

![image](https://user-images.githubusercontent.com/114064027/193798216-b8bf2b65-730e-4d21-ae77-3a708d329bc1.png)

**Количество участков N = 32

Количество N = 2023**





