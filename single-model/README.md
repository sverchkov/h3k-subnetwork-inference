# The single-model subnetwork inference pipeline

The makefile runs the pipeline

* Input files
    * Differential expression data
        * Matrix of knockout-molecule log-odds of differential expression, or
        * List of knockout-molecule differential expression calls
    * List of knockout-enzyme edges
    * List of enzyme-reaction edges
    * List of reaction-metabolite edges
    
* Candidate path generation
    * Find all paths from each KO to its respective molecule
    * Write corresponding GAMS constraints

* Solve integer problem which:
    * In the binary call case:
        * Connects all KOs to their respective (reachable) hits
        * Subject to that, Minimizes nodes used
    * In the log-odds case:
        * Maximizes sum of active path log-odds
        
## GAMS Program Details

#### The output of the pathfinder

* `ppi(edge)` bidirectional edges
* `fwd(edge,path)` undirected edges that proceed forward in paths
* `back(edge,path)` undirected edges that proceed backward in paths
* `pstart(node, path)` for each node the list of paths that start at it
* `pnode(node, path)` for each node the list of paths that contain it
* `pedge(node, path)` for each edge the list of paths that contain it

#### Our objective function

We want to maximize
sum over (source,target) {lods(source, target) * reachable(source, target)}
where:

reachable(source,target) : binary and =
max over path { pnode(target, path) * pstart(target, path) * sigma(path) }
