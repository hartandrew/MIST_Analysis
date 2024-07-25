The CITEseq dataset contains both single cell 10X 3' RNA and data for ~100 Abs from TotalSeq B Mouse kit (Biolegend). Samples include multiple intestinal niches - the ileum draining mesenteric lymph node, the ileum lamina propria, and the epithelial layer of the ileum. Infections/conditions include Naive, Cryptosporidium parvum (4 day past infection), Yersinia pseudotuberculosis (5 days post infection), and Heligmosomoides polygyrus (2 weeks post infection).

CellRanger alignment output for each sample was loaded into R and filtered. RNA and protein data were independently filtered to median +/- 2.5 median absolute deviations of the log10 transformed total reads and the RNA dataset was filtered to contain <20% mitochondrial reads per cell. Different samples were then combined into two large datasets (RNA/Protein). 

RNA and Protein datasets were independently normalized, scaled, and integrated. RNA data were subjected to cell cycle scoring and regression (removing dominant effects of cell cycle). Afterward, the data were combined once more for Weighted Nearest Neighbor analysis, clustering, and Umap generation utilizing both RNA and protein data. Doublets were scored and then removed before repeating clustering and umap generation. This final UMAP is the one seen on cellxgene.

Annotations of cell types were performed using several methods. First, SingleR was used to automate labeling of WNN-defined clusters.  A second automated cell labeling package (Azimuth) was also utilized. You can find the output of both in the CellxGene object. These provided good labels for many cell populations but the references lack appropriate epithelial cell types and thus epithelial cells were manually labeled using published subset markers. For example – Tuft cells were defined by Dclk1, Gnat3, Plcb2, Trpm5, Il25, and Tslp. Further annotation and refinement was performed manually. CellxGene was used to interactively visualize cell types to assess additional potential subsets and the accuracy of automatic labels. The final working cell labels are found under “CellTypes” though these could be further modified, adjusted, and corrected as we progress. 

Description of files in Scripts 

Processing_01_reduced.Rmd - This is an R markdown file containing the code used to input Cellranger output files into R, to filter samples based on total reads/genes and mitochondrial percent, add relevant metadata, normalize protein level data, and generate a combined seurat object containing all samples and both RNA and Protein level assays.

Primary_analysis_02.Rmd - This is an R markdown file containing the code to perform the bulk of the analysis as described in the paragraphs above (normalization, scaling, integration, clustering, annotation, UMAPs) as well as additional visualization and exploration.

Crypto_scData.Rmd - This is an R markdown file which utilizes separate Cellranger output. For this analysis, the sequences were run through cellranger aligment using the Cryptosporidium genome reference to map reads which would have come from cells infected with Cryptosporidium. In the R markdown file, this output is loaded and filtered and the barcode identities of the infected cells were output/saved and later used in "Primary_Analysis_02.Rmd" to identify and label cells that were infected with Cryptosporidium. As expected, these are epithelial cells of the intestinal epithelial cell compartment. 

Description of CellxGene field available

Azimuth_labels_l2 – Azimuth (an R package) automated cell annotation output (mid level specificity)

CellType – The curated and working cell type definitions based on combining automated labeling, manual scoring and manual visualization of populations

Crypto_infected_cells – Literally the cells which have substantial cryptosporidium-mapping reads – ie the cell is/is not infected with cryptosporidium

InfectionStatus – What was the mouse condition (Naïve, yersinia infected, crypto infected)

Mouse – Shows the individual mouse – useful for comparing between two different replicates of an infection model

Phase – The cell cycle phase assigned to a cell by Seurats cellcycle scoring function

SingleR.labels.wnn – The SingleR package’s automated cell type labels for each of the clusters - Immgen RNA set data from the package celldex were used as references

Tissue – What anatomical niche is represented by the sample (ileum draining MLN, ileum LP, ileum epithelial layer)

WNN_clusters1_cc – These are the original clusters which were defined. Notably some of them have been separated if they appeared to contain multiple cell types or they have been combined if they represent one larger cell type.  – This is useful alternative to looking at the cells by defined cell type. 

Orig.Ident – The mouse/tissue combination that identifies a sample

Ncount_RNA – Total reads per cell – not useful because they all cluster at the bottom

nFeature_RNA – The number of genes identified per cell

rna.size – a transformed version of the total RNA transcript reads per cell which accurately reflects the scale on cellxgene

prot.size - a transformed version of the total AB-linked reads per cell which accurately reflects the scale on cellxgene

n.gene – same as nfeature_RNA 

mt.prop – the proportion of mitochondrial reads – mostly a quality control unless someone has a specific interest

Epithelial Pseudotime – Monocle generated pseudo time of the epithelial compartment – Useful for identifying the more mature (apical villus) enterocyte cells from less mature cells

