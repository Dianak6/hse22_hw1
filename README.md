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


