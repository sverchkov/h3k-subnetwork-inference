The single-model subnetwork inference pipeline
=====

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