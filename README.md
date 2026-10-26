# Task 4 – Netflix Content Segmentation

Unsupervised learning: group Netflix titles into meaningful **clusters** with K-Means. There is no target to predict; the algorithm discovers the groups and we interpret them.

## Method
1. **Prepare numerical and categorical features**: numerical columns (release year, year added, delay before addition, movie length, number of seasons, number of genres) are standardized; rating group and main country are One-Hot encoded; genres are multi-hot encoded after removing words that reveal the type ("TV", "Movies"...), so clusters reflect themes rather than just Movie vs TV Show
2. **Apply clustering algorithms**: K-Means, with the number of clusters chosen using the elbow method (inertia) and the silhouette score
3. **Identify content groups**: a profile per cluster (size, share of TV shows, typical length, release year, top rating, top country, characteristic genres) and automatic starting names
4. **Visualize clusters**: PCA projection with centroids, cluster sizes, genre heatmap per cluster
5. **Interpret cluster characteristics**: z-score heatmap versus the catalogue average, representative titles closest to each centre, stability check with the Adjusted Rand Index

## Key points
- K-Means is distance-based, so features must be numeric and on a comparable scale (hence the standardization and encoding).
- Genres are normalized so that "Dramas" and "TV Dramas" count as the same theme.
- The silhouette optimum (k = 3) only separates TV shows, movies and older classic movies, which is too coarse. **k = 5** was chosen for readable, distinct groups (silhouette 0.171 vs 0.217 at k = 3). It can be changed with `K = ...`.
- The clustering is validated by re-running K-Means with different seeds (Adjusted Rand Index close to 1 = stable groups).

## Results
8,787 titles, 5 clusters (k = 5), silhouette score 0.171, mean Adjusted Rand Index 0.852 (reasonably stable).

| Cluster | Titles | TV show (%) | Characteristic genres | Suggested name |
|---|---|---|---|---|
| 0 | 456 (5%) | 100 | Kids', Reality, Comedies, Crime | Kids and reality TV |
| 1 | 3,331 (38%) | 0 | International, Dramas, Comedies, Independent | International dramas and comedies (movies) |
| 2 | 611 (7%) | 5 | Classic, Cult, Action & Adventure, Dramas | Classic and cult movies (median release year 1992) |
| 3 | 2,517 (29%) | 13 | Documentaries, Stand-Up Comedy, Children & Family | Documentaries, stand-up and family (shorter, median 87 min) |
| 4 | 1,872 (21%) | 99 | International, Crime, Docuseries, British | International crime and docuseries TV |

Movies split into three groups (international dramas, classics, documentaries and stand-up) and TV shows into two (international crime and docuseries, kids and reality). Classic movies are the most distinct group in time (older release years), while the other groups overlap in the PCA view because they share similar years and ratings.

## Output
The notebook also saves `netflix_clusters.csv` (title, type, cluster, cluster name).

## Run the project
Put `Dataset.csv` in the same folder as the notebook, then:
```bash
pip install -r requirements_task4.txt
jupyter notebook task4_content_segmentation.ipynb
```
