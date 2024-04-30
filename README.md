# trop_niger_fst_2024

I calculated FST as described in

https://github.com/TharinduTS/trop_pop_gen_2023?tab=readme-ov-file#fst

and then plotted data with following r script
```R
library("rstudioapi") 
setwd(dirname(getActiveDocumentContext()$path))

library(ggplot2)

# **************combination1************************
#read table and rename columns
fst_data<-read.table(file = "./sliding_win_files/slidingwindow_com1",sep = '\t',row.names =NULL)
colnames(fst_data)<-c('loc_info','chr','win','mid_pos','fst')

#create a column combining chr and loc

fst_data$full_loc<-paste(fst_data$chr,fst_data$mid_pos,sep = '-')

chr_order<-c('Chr1','Chr2','Chr3','Chr4','Chr5','Chr6','Chr7','Chr8','Chr9','Chr10')

# selecting only chromosomes leaving rest

fst_data_chrs_only<-subset(fst_data,grepl("Chr", fst_data$chr))

#changing column order to make it look better
fst_data_chrs_only$chr <- factor(fst_data_chrs_only$chr,levels = chr_order)


# **************combination2************************

#read table and rename columns
fst_data_2<-read.table(file = "./sliding_win_files/slidingwindow_com2",sep = '\t',row.names =NULL)
colnames(fst_data_2)<-c('loc_info','chr','win','mid_pos','fst')


#create a column combining chr and loc

fst_data_2$full_loc<-paste(fst_data_2$chr,fst_data_2$mid_pos,sep = '-')


# selecting only chromosomes leaving rest

fst_data_chrs_only_2<-subset(fst_data_2,grepl("Chr", fst_data_2$chr))

#changing column order to make it look better
fst_data_chrs_only_2$chr <- factor(fst_data_chrs_only_2$chr,levels = chr_order)

#plot

p<-ggplot(fst_data_chrs_only,aes(x=win/1000000,y=fst))+
  #geom_point(alpha=1)+
  geom_smooth()+
  geom_smooth(data = fst_data_chrs_only_2,color='red')+
  #labs(title = expression(paste(italic(F[ST]),"  ","Ghana only","    -      Chr7")))+
  #ylim(-0.05, 0.25)+
  theme_bw()+
  xlab("Chrom_position(mb)")+
  ylab(expression(paste(italic(F[ST]))))+
  theme_classic()+
  facet_wrap(~chr,nrow = 10)+
  ylim(0,1)

ggsave("fst_plot.pdf",p,width = 10,height = 15)
```
