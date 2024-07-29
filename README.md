> The CITE-seq dataset contains both single cell 10X 3' RNA and data for ~100 Abs from TotalSeq B Mouse kit (Biolegend). Samples include multiple intestinal niches - the ileum draining mesenteric lymph node, the ileum lamina propria, and the epithelial layer of the ileum. Infections/conditions include Naive, *Cryptosporidium parvum* (4 day past infection), *Yersinia pseudotuberculosis* (5 days post infection), and *Heligmosomoides polygyrus* (2 weeks post infection).

# Methods

## Preprocessing
CellRanger alignment output for each sample was loaded into R and filtered. RNA and protein data were independently filtered to median +/- 2.5 median absolute deviations of the log10 transformed total reads and the RNA dataset was filtered to contain <20% mitochondrial reads per cell. Different samples were then combined into two large datasets (RNA/Protein). 

RNA and Protein datasets were independently normalized, scaled, and integrated. RNA data were subjected to cell cycle scoring and regression (removing dominant effects of cell cycle). Afterward, the data were combined once more for Weighted Nearest Neighbor analysis, clustering, and Umap generation utilizing both RNA and protein data. Doublets were scored and then removed before repeating clustering and umap generation. This final UMAP is the one seen on cellxgene.

## Cluster annotation
Annotations of cell types were performed using several methods. First, SingleR was used to automate labeling of WNN-defined clusters.  A second automated cell labeling package (Azimuth) was also utilized. You can find the output of both in the CellxGene object. These provided good labels for many cell populations but the references lack appropriate epithelial cell types and thus epithelial cells were manually labeled using published subset markers. For example – Tuft cells were defined by Dclk1, Gnat3, Plcb2, Trpm5, Il25, and Tslp. Further annotation and refinement was performed manually. CellxGene was used to interactively visualize cell types to assess additional potential subsets and the accuracy of automatic labels. The final working cell labels are found under “CellTypes” though these could be further modified, adjusted, and corrected as we progress. 

# Description of files in this repository

- TotalSeq_B_Universal_Cocktail_v1_140_Antibodies_399904_Barcodes.xlsx - This Excel spreadsheets lists all antibody clones used to labels cells in this study.  More details about the BioLegend Total Seq B mouse antibody panel can be found [here](https://www.biolegend.com/fr-ch/products/totalseq-b-human-universal-cocktail-v1dot0-20960).
- Scripts/Processing_01_reduced.Rmd - This is an R markdown file containing the code used to input Cellranger output files into R, to filter samples based on total reads/genes and mitochondrial percent, add relevant metadata, normalize protein level data, and generate a combined seurat object containing all samples and both RNA and Protein level assays.
- Scripts/Primary_analysis_02.Rmd - This is an R markdown file containing the code to perform the bulk of the analysis as described in the paragraphs above (normalization, scaling, integration, clustering, annotation, UMAPs) as well as additional visualization and exploration.
- Scripts/Crypto_scData.Rmd - This is an R markdown file which utilizes separate Cellranger output. For this analysis, the sequences were run through cellranger alignment using the Cryptosporidium genome reference to map reads which would have come from cells infected with Cryptosporidium. In the R markdown file, this output is loaded and filtered and the barcode identities of the infected cells were output/saved and later used in "Primary_Analysis_02.Rmd" to identify and label cells that were infected with Cryptosporidium. As expected, these are epithelial cells of the intestinal epithelial cell compartment. 

# Description of CellxGene fields available

- Cell Cycle Phase – The cell cycle phase assigned to a cell by Seurats cellcycle scoring function
- Cell Type (curated) – The curated and working cell type definitions based on combining automated labeling, manual scoring and manual visualization of populations
- InfectionStatus – What was the mouse condition (Naïve, yersinia infected, crypto infected)
- Mouse – Shows the individual mouse – useful for comparing between two different replicates of an infection model
- Tissue – What anatomical niche is represented by the sample (ileum draining MLN, ileum LP, ileum epithelial layer)
- WNN_clusters1_cc - these are the original weighted nearest neighbor clusters assigned by Seurat
- Orig.Ident – The mouse/tissue combination that identifies a sample
- nFeature_RNA – The number of genes identified per cell
- rna.size – a transformed version of the total RNA transcript reads per cell which accurately reflects the scale on cellxgene
- prot.size - a transformed version of the total AB-linked reads per cell which accurately reflects the scale on cellxgene
- n.gene – same as nfeature_RNA 
- mt.prop – the proportion of mitochondrial reads – mostly a quality control unless someone has a specific interest
- Epithelial Pseudotime – Monocle generated pseudo time of the epithelial compartment – Useful for identifying the more mature (apical villus) enterocyte cells from less mature cells

