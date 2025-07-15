# Filogenia del Gen rbcl de la familia Arecaceae

Luis Gutiérrez (lagutierrez@puce.edu.ec)
Junio 26, 2025

Este programa permite hacer filogenias a partir de la base de datos de NCBI. 
Aplicable a cualquier ejercicio con los mismos principios y fines, se logra 
la evolución y la presencia o no de estos genes en las especies de esta familia
Arecaceae.

La generación de esta filogenias nos permite materializar de alguna manera la historia
evolutiva de este gen que representa la subunidad grande de la ribulosa-1,5-bisfosfato
carboxilasa/oxigenasa. Este gen cloroplástico capaz de codificar la RuBisCo es esencial
en la fotosíntesis. Esta subunidad cataliza la fijación del CO2 durante el ciclo 
de Calvin, así este gen en relevante en la conversión del dióxido de carbono en compuestos
orgánicos esenciales para el desarrollo de las plantas (Ellis, 2010).

En palmeras el gen rbcl permite que estas especies se adapten a ambientes diferentes
Este gen ha sido útil para demostrar su diversificación en esta familia de plantas
y su evolución gracias ha que es alramente conservado (Baker et al., 2009).

![alt text](https://www.palmpedia.net/wiki/images/thumb/3/31/2199.jpg/800px-2199.jpg)
Ejemplar de la palma de la chonta

## Procedimiento

Prerequisitos para ejecutar el programa:

* Conexión con un supercomputador en este caso Hoffman2
* Muscle
* IQTREE
* FigTree (http://tree.bio.ed.ac.uk/software/figtree/)
* EDirect

Flujo de trabajo del programa:

- Importar las secuencias de NCBI con EDirect
- Alinear las secuencias importadas desde NCBI (MUSCLE)
- Descargar el output del alineamiento 
- Ejecutar las filogenias posibles (IQTREE)
- Finalmente visualizar las filogenias en un árbol (FIGTREE)

Instrucciones:

1. Conectarse al supercomputador Hoffman2
2. Instalar EDirect
sh -c "$(curl -fsSL https://ftp.ncbi.nlm.nih.gov/entrez/entrezdirect/install-edirect.sh)"
- Verificar su instalación con:
esearch -version
3. Descargar las secuencias buscando IDs de secuencias en NCBI y 
guardar el resultado como .txt
esearch -db nucleotide -query "Arecaceae[Organism] AND rbcL[Gene] AND 500:3000[Sequence Length]" | \
efetch -format uid > arecaceae_rbcl_ids.txt
- Para hacer la descarga en formato FASTA:
efetch -db nucleotide -id $(cat arecaceae_rbcl_ids.txt) -format fasta > arecaceae_rbcl.fasta
4. Simplemente para verificar las secuencias descargadas:
grep ">" arecaceae_rbcl.fasta
5. Alinear las secuencias con MUSCLE:
./muscle3.8.31_i86linux64 -in arecaceae_rbcl.fasta -out arecaceae_rbcl.alineamiento.fasta -maxiters 1 -diags
6. Crear las filogenias con IQTREE:
- Primero se instala en programa con:
module load iqtree/2.2.2.6
- Para ejecutar el programa con el alinemiaento previo:
iqtree2 -s arecaceae_rbcl.alineamiento.fasta
- La ejecuación previa nos dará como resultado 4 archivos nuevos a partir de arecaceae_rbcl.alineamiento.fasta
- De todos los archivos que nos dión esta ejecución trabajemos únicamente con los 
que terminen en .treefile
6. Visualizar las filogenias con FIGREE:
- Primero se debe instalar el programa en el siguiente link: http://tree.bio.ed.ac.uk/software/figtree/
- Descargar el archivo .treefile al escritorio de tu ordenador con scp ayudandote de pwd
- Abrir el programa y abrir el .treefile
- Ahora podrás ver el resultado final de la ejecución de todo este programa



--------------------------------------------------------------------------------
## Herramientas y Tips Extras

###Para verificar el alineamiento con MESQUITE

- Para ejecutar el programa necesitas Java9
- Instalar el programa en tu ordenador https://www.mesquiteproject.org/
- Descargar el archivo .alineamiento.fasta al escritorio de tu ordenador con scp
- Abrir el MESQUITE y subir el archivo .alineamiento.fasta
- Seleccionar FASTA (DNA/RNA)
- Ahora podrás ver el alineamiento

### En el caso de que tu filogenia no salgan los nombre de las especies

- Antes de ejecutar el iqtree deberás modificar el archivo input, de tal manera
que cambies los espacios en los nombres por "_"
- Ayudate de este comando:
sed 's/ /_/g' arecaceae_rbcl.alineamiento.fasta > arecaceae_rbcl-alineamiento.clean.fasta
- Ahora sí, podras bajar el .treefile del .alineamiento.clean.fasta y ver tu filogenia
en Figtree

### Para modificar la presentación de los taxones en la filogenia

- Al descargar el archivo treefile luego de ejecutar iqtree y subirlo al figtree
se logra ver toda la información descargada desde la base de datos del NCBI, sin 
embargo si desea modificar esto de tal manera que en la filogenia solo se vea
las especies en cada rama puede modificar este texto con el programa Atom - https://atom-editor.cc/




Referencias:
-  Edgar, R.C. Nucleic Acids Res 32(5), 1792-97.
- Baker, W. J., Savolainen, V., Asmussen-Lange, C. B., Chase, M. W., Dransfield,
J., Forest, F., Harley, M. M., Uhl, N. W., & Wilkinson, M. (2009). Complete generic-level
phylogenetic analyses of palms (Arecaceae) with comparisons of supertree and supermatrix 
approaches. Systematic biology, 58(2), 240–256. https://doi.org/10.1093/sysbio/syp021
- Ellis, J. R. (2010). The most abundant protein on Earth. Trends in Biochemical Sciences, 
35(6), 322-324.

