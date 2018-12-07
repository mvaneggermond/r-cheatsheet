Thousand seperator

library(scales)
scale_y_continuous(labels=function(x) format(x, big.mark = ",", scientific = FALSE)) 
Bold legend

theme(legend.position="right",legend.title = element_text(face="bold")) + 

Limits on axis

scale_y_continuous(limits = c(0, 0.8)) +

Percentage

scale_y_continuous(labels = scales::percent) +

