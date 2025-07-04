# Filogenia del Gen (---) de las Palmas del Ecuador

Luis Gutiérrez (lagutierrez@puce.edu.ec)
Junio 26, 2025

Este programa permite hacer filogenias a partir de la base de datos de NCBI. 
Aplicable a cualquier ejercicio con los mismos principios y fines se logra 
la evolución y la presencia o no de estos genes en las especies de la
Arecaceae

![alt text](https://www.palmpedia.net/wiki/images/thumb/3/31/2199.jpg/800px-2199.jpg)

## Procedimiento

Prerequisitos para ejecutar el programa:

* Conexión con un supercomputador en este caso Hoffman2
* Muscle
* Blast
* FigTree
* EDirect

Flujo de trabajo del programa:

- Importar las secuencias de NCBI con EDirect
- Alinear las secuencias importadas desde NCBI (MUSCLE)
- Descargar el output del alineamiento 
- Ejecutar las filogenias posibles (IQTREE)
- Finalmente crear la filogenia (FIGTREE)

Instrucciones:

1.  Instalar EDirect
sh -c "$(curl -fsSL https://ftp.ncbi.nlm.nih.gov/entrez/entrezdirect/install-edirect.sh)"
- Verificar su instalación con:
esearch -version
2. Para descargar las secuencias buscando IDs de secuencias en NCBI
esearch -db nucleotide -query "Arecaceae[Organism] AND rbcL[Gene] AND 500:3000[Sequence Length]" | \
efetch -format uid > arecaceae_rbcl_ids.txt
- Para hacer la descarga en formato FASTA:
efetch -db nucleotide -id $(cat arecaceae_rbcl_ids.txt) -format fasta > arecaceae_rbcl.fasta
3. Simplemente para verificar las secuencias descargadas:
grep ">" arecaceae_rbcl.fasta
4. Para alinear las secuencias con MUSCLE:
