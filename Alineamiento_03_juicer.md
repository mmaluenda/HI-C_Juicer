# Guía para alineamiento 03, 04, 05, 06, 07

```Juicer``` es un pipeline automatizado desarrollado por el grupo de Dekker y Aiden Labs, diseñado para procesar datos de ```Hi-C``` desde los archivos ```FASTQ``` hasta los mapas de contacto ```.hic```. Lo hace todo: alineamiento, filtrado, deduplicación, construcción del grafo de contacto, generación de mapas ```.hic```, etc.

Antes iniciar el protocolo debemos entrar al ambiente de conda ```samtools``` ya que ```juicer``` lo ocupa cono una de sus librerias principales luego de realizar el alineamiento. Además, necesita archivos de entrada con formato archivos ```*.fastq.gz``` emparejados (R1, R2), un archivo de restricción, un archivo ```.chrom.sizes``` y un archivo ```*.fa``` que corresponde al gend de referencia.

```Juicer``` en todo su preprocesamiento hace trimming automático de adaptadores (parcialmente), fragmentos de sitios de restricción y también elimina ligaciones fantasma o artefactos.

Alineamiento con ```BWA```: Usa ```bwa mem``` para alinear cada read, alinea ambos pares (R1, R2), guarda archivos intermedios con formatos ```*.sam``` y ```*.bam```. Filtrado de pares: elimina duplicados, pares mal alineados o unilaterales, pares de baja calidad. 
```bash
$ conda activate samtools
```
```bash
$ /home/proyectos/HIC_03_Nov_2023/outputs/03_juicer/juicedir/scripts/juicer.sh -z /home/proyectos/HIC_03_Nov_2023/outputs/03_juicer/juicedir/references/mm10.fa -p /home/proyectos/HIC_03_Nov_2023/outputs/03_juicer/juicedir/references/sizes.genome -s lab -d . -D /home/resources/programs/juicer -y /home/proyectos/HIC_03_Nov_2023/outputs/03_juicer/juicedir/restriction_sites/mm10_lab.txt
```
el parametro -z se refiere al gen de referencia que esta ocupando en este caso es ```mm10.fa```, -p el mapa de tamaño del genoma, -s (```lab```) es el gen de restricción. 

Generación de archivos ```.hic```, contiene todas las matrices de contacto a distintas resoluciones (5kb, 10kb, 50kb, etc.). outputs: ```*inter.hic``` archivo binario con el contacto cromosómico, ```../aligned/``` directorio con los archivos ```BAM``` intermedios.
