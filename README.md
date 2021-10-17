# Домашнее задание №1
Список всех команд для первой части задания:
```
ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oil_R2.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq

seqtk sample -s1009 oil_R1.fastq 5000000 > sub_R1.fq
seqtk sample -s1009 oil_R2.fastq 5000000 > sub_R2.fq
seqtk sample -s1009 oilMP_S4_L001_R1_001.fastq 1500000 > sub_MP_R1.fq
seqtk sample -s1009 oilMP_S4_L001_R2_001.fastq 1500000 > sub_MP_R2.fq

mkdir fastqc
ls sub* | xargs -tI{} fastqc -o fastqc {}
mkdir multiqc
multiqc -o multiqc fastqc

platanus_trim sub_R1.fq sub_R2.fq
platanus_internal_trim sub_MP_R1.fq sub_MP_R2.fq

rm sub_R1.fq
rm sub_R1.fq
rm sub_MP_R1.fq
rm sub_MP_R2.fq

mkdir fastqc_trimmed
ls sub* | xargs -tI{} fastqc -o fastqc_trimmed {}
mkdir multiqc_trimmed
multiqc -o multiqc_trimmed fastqc_trimmed

platanus assemble -f sub_R1.fq.trimmed sub_R2.fq.trimmed 2> logfile.log
platanus scaffold -c out_contig.fa -IP1 sub_R1.fq.trimmed sub_R2.fq.trimmed -OP2 sub_MP_R1.fq.int_trimmed sub_MP_R2.fq.int_trimmed 2> scaffold.log
platanus gap_close -c out_scaffold.fa -IP1 sub_R1.fq.trimmed sub_R2.fq.trimmed -OP2 sub_MP_R1.fq.int_trimmed sub_MP_R2.fq.int_trimmed 2> gapclose.log

head out_gapClosed.fa
echo scaffold1_cov231 > seq_names.lst
seqtk subseq out_gapClosed.fa seq_names.lst > longest.fa
```
Вторая часть:
```
seqtk sample -s1009 oil_R1.fastq 3750000 > sub_75_R1.fq
seqtk sample -s1009 oil_R1.fastq 2500000 > sub_50_R1.fq
seqtk sample -s1009 oil_R1.fastq 1250000 > sub_25_R1.fq
seqtk sample -s1009 oil_R2.fastq 3750000 > sub_75_R2.fq
seqtk sample -s1009 oil_R2.fastq 2500000 > sub_50_R2.fq
seqtk sample -s1009 oil_R2.fastq 1250000 > sub_25_R2.fq
seqtk sample -s1009 oilMP_S4_L001_R1_001.fastq 1125000 > sub_MP_75_R1.fq
seqtk sample -s1009 oilMP_S4_L001_R1_001.fastq 750000 > sub_MP_50_R1.fq
seqtk sample -s1009 oilMP_S4_L001_R1_001.fastq 375000 > sub_MP_25_R1.fq
seqtk sample -s1009 oilMP_S4_L001_R2_001.fastq 1125000 > sub_MP_75_R2.fq
seqtk sample -s1009 oilMP_S4_L001_R2_001.fastq 750000 > sub_MP_50_R2.fq
seqtk sample -s1009 oilMP_S4_L001_R2_001.fastq 375000 > sub_MP_25_R2.fq

platanus_trim sub_75_R1.fq sub_75_R2.fq
platanus_internal_trim sub_MP_75_R1.fq sub_MP_75_R2.fq
platanus_trim sub_50_R1.fq sub_50_R2.fq
platanus_internal_trim sub_MP_50_R1.fq sub_MP_50_R2.fq
platanus_trim sub_25_R1.fq sub_25_R2.fq
platanus_internal_trim sub_MP_25_R1.fq sub_MP_25_R2.fq

platanus assemble -o 75 -f sub_75_R1.fq.trimmed sub_75_R2.fq.trimmed 2> logfile.log
platanus assemble -o 50 -f sub_50_R1.fq.trimmed sub_50_R2.fq.trimmed 2> logfile.log
platanus assemble -o 25 -f sub_25_R1.fq.trimmed sub_25_R2.fq.trimmed 2> logfile.log
platanus scaffold -o 75 -c 75_out_contig.fa -IP1 sub_75_R1.fq.trimmed sub_75_R2.fq.trimmed -OP2 sub_MP_75_R1.fq.int_trimmed sub_MP_75_R2.fq.int_trimmed 2> scaffold.log
platanus scaffold -o 50 -c 50_out_contig.fa -IP1 sub_50_R1.fq.trimmed sub_50_R2.fq.trimmed -OP2 sub_MP_50_R1.fq.int_trimmed sub_MP_50_R2.fq.int_trimmed 2> scaffold.log
platanus scaffold -o 25 -c 25_out_contig.fa -IP1 sub_25_R1.fq.trimmed sub_25_R2.fq.trimmed -OP2 sub_MP_25_R1.fq.int_trimmed sub_MP_25_R2.fq.int_trimmed 2> scaffold.log
platanus gap_close -o 75 -c 75_out_scaffold.fa -IP1 sub_75_R1.fq.trimmed sub_75_R2.fq.trimmed -OP2 sub_MP_75_R1.fq.int_trimmed sub_MP_75_R2.fq.int_trimmed 2> gapclose.log
platanus gap_close -o 50 -c 50_out_scaffold.fa -IP1 sub_50_R1.fq.trimmed sub_50_R2.fq.trimmed -OP2 sub_MP_50_R1.fq.int_trimmed sub_MP_50_R2.fq.int_trimmed 2> gapclose.log
platanus gap_close -o 25 -c 25_out_scaffold.fa -IP1 sub_25_R1.fq.trimmed sub_25_R2.fq.trimmed -OP2 sub_MP_25_R1.fq.int_trimmed sub_MP_25_R2.fq.int_trimmed 2> gapclose.log
```
Скриншоты из multiQC:    
Для исходных чтений:
![image](https://user-images.githubusercontent.com/55440084/137643802-62cb4660-41d2-4ae2-84bc-e9388b960b88.png)
![image](https://user-images.githubusercontent.com/55440084/137643810-470108f7-2e9d-45fc-a20e-30748e7d38cc.png)
![image](https://user-images.githubusercontent.com/55440084/137643822-a3f57e57-82ec-4e8b-b5fa-bfd62b9d55b5.png)
![image](https://user-images.githubusercontent.com/55440084/137643838-aed00cfc-7f19-4dff-8de6-1efe81f07bbe.png)
![image](https://user-images.githubusercontent.com/55440084/137643852-62c14cf7-3785-4289-b0bf-7860f91793a7.png)

Для подрезанных чтений:
![image](https://user-images.githubusercontent.com/55440084/137643860-7369d064-13bd-4d37-bea7-00ecce8b5264.png)
![image](https://user-images.githubusercontent.com/55440084/137643877-86d50002-f164-48ad-b161-086cf6120850.png)
![image](https://user-images.githubusercontent.com/55440084/137643890-e5775d6f-6db3-4ede-b31b-969d50b1e158.png)
![image](https://user-images.githubusercontent.com/55440084/137643900-584f04be-b233-46ca-ba9a-ee9223ccea75.png)
![image](https://user-images.githubusercontent.com/55440084/137643920-1585c9ca-58d7-49fc-bb3b-9ff5badc4e8c.png)

[Ссылка](https://colab.research.google.com/drive/1UOS29IMDMfFxz16yU4snxUtTZ3M7d1H6?usp=sharing) на Google colab. Там же присутствует решение второй части.  
Код:
```
!pip install biopython
from Bio import SeqIO
import re
import matplotlib.pyplot as plt

def collect_info(records):
  total = sum(len(record) for record in records)
  longest = len(records[0])
  score = 0
  n_50 = 0
  ind = 0
  while(score < total/2):
    n_50 = len(records[ind])
    ind += 1
    score += n_50
  print(f'\tTotal length: {total}')
  print(f'\tThe longest contig size: {longest}')
  print(f'\tN50: {n_50}')

def measure_gaps(record):
  matches = re.findall(r'[N]+', str(record.seq))
  N_repetitions = str(record.seq).count('N')
  print(f'\tGaps in total: {len(matches)}')
  print(f'\tGap symbols in total: {N_repetitions}')
  print(f'\tCheck: {sum(len(c) for c in matches)} == {N_repetitions}')

def gaps_total(records):
  return sum(len(re.findall(r'[N]+', str(record.seq))) for record in records)

def plot_comparings(x, y, graph, desc, general_desc):
  for i in range(len(y)):
    graph.plot(x, y[i], label=desc[i])
  graph.title.set_text(general_desc)
  graph.set_xlabel('Size of subset (% from part 1)')
  graph.set_ylabel('In total')
  graph.legend()


first_records = list(SeqIO.parse("out_contig.fa", "fasta"))
scaffold_records = list(SeqIO.parse("out_scaffold.fa", "fasta"))
gap_Closed_records = list(SeqIO.parse("out_gapClosed.fa", "fasta"))
first_records.sort(key=lambda r: -len(r))
scaffold_records.sort(key=lambda r: -len(r))
gap_Closed_records.sort(key=lambda r: -len(r))

SeqIO.write([scaffold_records[0]], 'longest.fa', "fasta")

print(f' - - Contigs in total: {len(first_records)} - - - - - - - - - -')
collect_info(first_records)

print(f'\n - - Scaffold records in total: {len(scaffold_records)} - - - - - - - - - -')
collect_info(scaffold_records)

print(f'\n - - Scaffold the longest contig - - - - - - - - - - - - - - - - - - - - -')
measure_gaps(scaffold_records[0])

print(f'\n - - Gap closed scaffold the longest contig - - - - - - - - - - - - - - -')
measure_gaps(gap_Closed_records[0])

print('\n~ ~ ~ ~ ~ ~ ~ ~ ~ ~ Part 2 ~ ~ ~ ~ ~ ~ ~ ~ ~ ~\n')

scaffold_records_75 = list(SeqIO.parse("75_scaffold.fa", "fasta"))
gap_Closed_records_75 = list(SeqIO.parse("75_gapClosed.fa", "fasta"))
scaffold_records_50 = list(SeqIO.parse("50_scaffold.fa", "fasta"))
gap_Closed_records_50 = list(SeqIO.parse("50_gapClosed.fa", "fasta"))
scaffold_records_25 = list(SeqIO.parse("25_scaffold.fa", "fasta"))
gap_Closed_records_25 = list(SeqIO.parse("25_gapClosed.fa", "fasta"))
scaffold_x = [25, 50, 75, 100]
scaffold_contigs_y = [len(scaffold_records_25), len(scaffold_records_50), len(scaffold_records_75), len(scaffold_records)]
scaffold_gaps_y = [gaps_total(scaffold_records_25), gaps_total(scaffold_records_50), gaps_total(scaffold_records_75), gaps_total(scaffold_records)]
closed_scaffold_contigs_y = [len(gap_Closed_records_25), len(gap_Closed_records_50), len(gap_Closed_records_75), len(gap_Closed_records)]
closed_scaffold_gaps_y = [gaps_total(gap_Closed_records_25), gaps_total(gap_Closed_records_50), gaps_total(gap_Closed_records_75), gaps_total(gap_Closed_records)]

fig, axs = plt.subplots(figsize=(10, 7), ncols=2)
fig.suptitle('Dependence of size of subsets on number of scaffolds and gaps', y=1.02, fontweight="bold")
plot_comparings(scaffold_x, [scaffold_contigs_y], axs[0], ['Number of scaffolds'], 'Comparison of the number of scaffolds')
plot_comparings(scaffold_x, [scaffold_gaps_y, closed_scaffold_gaps_y], axs[1], ['Number of gaps before \'gap_closed\'', 'Number of gaps after \'gap_closed\''], 'Comparison of the number of scaffolds')
plt.tight_layout()
```
Вывод:  
![image](https://user-images.githubusercontent.com/55440084/137644144-268fc7f3-6f15-45e9-9733-9e765361ba1f.png)
![image](https://user-images.githubusercontent.com/55440084/137644149-5553df37-b572-4b8b-891e-b315c1051103.png)
