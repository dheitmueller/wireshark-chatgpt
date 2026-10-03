# Static-analysis conventions from MRs 359-408

- Dead-store and dead-increment findings should be resolved against the parser cursor contract, not removed mechanically. MRs !369 and !370 show that related helpers should agree on whether they return an absolute next offset or a consumed length.
