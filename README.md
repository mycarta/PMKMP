PMKMP
=====

Matlab function to create perceptual colormaps, [as submitted on the Matlab File Exchange]. (http://www.mathworks.com/matlabcentral/fileexchange/28982-perceptually-improved-colormaps)

Function and supporting files and images licensed under the terms of [BSD] (https://opensource.org/licenses/BSD-2-Clause) license. 


function map=pmkmp(n,scheme)
 PMKMP Returns perceptually balanced colormaps with rainbow-like colors
   PMKMP(N,SCHEME) returns an Nx3 colormap. 
   usage: map=pmkmp(n,scheme);

 JUSTIFICATION: rainbow, or spectrum color schemes are considered a poor
 choice for scientific data display by many in the scientific community
 (see for example reference 1 and 2) in that they introduce artifacts 
 that mislead the viewer. "The rainbow color map appears as if it’s separated
 into bands of almost constant hue, with sharp transitions between hues. 
 Viewers perceive these sharp transitions as sharp transitions in the data,
 even when this is not the casein how regularly spaced (interval) data are
 displayed (quoted from reference 1). This submission is intended to share
 the results of my work to create more perceptually balanced, 
 rainbow-like color maps. Please see output arguments section for descriptions.


   arguments: (input)
   scheme - can be one of the following strings:
     'IsoL'      Lab-based isoluminant rainbow with constant luminance L*=60
                  For interval data displayed with external lighting

     'IsoAZ'      Lightness-Chroma-Hue based isoluminant rainbow going
                  around the full Hue circle.
                  For azimuthal and phase data.

     'LinearL'	  Lab-based linear lightness rainbow. 
                  For interval data displayed without external lighting
                  100 perceptual
 
     'LinLhot'	  Linear lightness modification of Matlab's hot color palette. 
                  For interval data displayed without external lighting
                  100 perceptual    

     'CubicYF'	   Lab-based rainbow scheme with cubic-law luminance(default)
                  For interval data displayed without external lighting
                  100 perceptual

     'CubicL'	   Lab-based rainbow scheme with cubic-law luminance
                  For interval data displayed without external lighting
                  As above but has red at high end (a modest deviation from
                  100 perceptual)

     'Edge'       Diverging Black-blue-cyan-white-yellow-red-black scheme
                  For ratio data (ordered, constant scale, natural zero)  

   n - scalar specifying number of points in the colorbar. Maximum n=256
      If n is not specified, the size of the colormap is determined by the
      current figure. If no figure exists, MATLAB creates one.


   arguments: (output)
   map - colormap of the chosen scheme
   - IsoL is based on work in paper 2 in the reference section.
     In both this paper and in several others this is indicated as the
     best for displaying interval data with external lighting.
     This is so as to allow the lighting to provide the shading to
     highlight the details of interest. If lighting is combined with a
     colormap that has its own luminance function associated - even as
     simple as a linear increase this will confuse the viewer. The only 
     difference from the paper is that I changed the value of constant 
     luminance to L*=60 to make it brighter that the authors' example.

   -  IsoAZ is a Lightness-Chroma-Hue based isoluminant rainbow that
      goes around the full Hue circle.For azimuthal and phase data.
      Created with code snippet below. This is a modification from an example
      by Steve Eddins on his Matlab central blog (reference 15). Steve had
      lightness increasing while as hue changed. I hold the ligthness
      constant instead to make the result isoluminant. I also use the 
      Colorspace transformations function instead of the Image Processing 
      Toolbox for the color conversion. Here's the code:

      radius = 50;   chroma
      theta = linspace(0, pi/2, 256)';   hue
      a = radius * cos(theta); 
      b = radius * sin(theta); 
      L = (ones(1, 256)*100)';   lightness
      Lab = [L, a, b]; 
      RGB=colorspace('RGB<-Lab',(Lab));  
       (needs Colorspace transformations from Matlab File Exchange)
      www.mathworks.com/matlabcentral/fileexchange/28790-colorspace-transformations

   - LinearL is a linear lightness modification of another palette from 
     paper 2 in the reference. For how it was generated see my blog post:
     mycarta.wordpress.com/2012/12/06/the-rainbow-is-deadlong-live-the-rainbow-part-5-cie-lab-linear-l-rainbow/
 
   - LinLhot is a linear lightness modification of Matlab's hot 
     color palette. For how it was generated see my blog post:
     mycarta.wordpress.com/2012/10/14/the-rainbow-is-deadlong-live-the-rainbow-part-4-cie-lab-heated-body/          

   - CubicL too is based on some of the ideas in paper 2 in the 
      reference section but rather than using a linearly increasing
      L* function such as the one used by those authors, I am
      using a compressive or cubic law function for the increase in 
      L*.  L* ranges between 31 and 90 in the violet to yellowish 
      portion of the colormap, then decreases to about 80 to get 
      to the red (please refer to figure L_a_b_PlotsCubicL.png).
      The choice to start at 31 was a matter of taste. 
      I like having violet instead of black at the cold end of the
      colormap. The latter choice was so as to have red and not
      white at the warm end  of the colorbar, which is also a 
      matter of personal taste. As a result,  there is an inversion in 
      the L* trend, but I believe because it is a smooth one that
      this is an acceptable compromise and the resulting
      colormap is much of an improvement over the standard 
      rainbow or spectrum schemes, which  typically have at least 3 sharp 
      L* inversions. Please run CompareLabPlotsUsingColorspace.m or see
      figures: L_plot_for_CubicL_colormap.png, L_plot_for_jet_colormap.png,
      and L_plot_for_spectrum_colormap.png for a demonstration

    - CubicYF A fully perceptual version of the above in which I eliminated
      the red tip at the high end. The work is described in papers 12 and 13. 
      I've uploaded 2 figures. The first, spectrum vs cubicYF.png, is a comparison
      of lightness versus sample number for the spectrum (top left) and the
      new color palette (bottom left), and also a comparison of test surface
      (again the Great Pyramid of Giza)using the spectrum (top right)and 
      the new color palette (bottom right). The second figure 
      simulations color vision deficieny.png
      is a comparison of spectrum and cubicYF rainbow for all viewers. 
      Left column: full color vision – for the spectrum (top left) and for the 
      cubeYF rainbow (bottom left). Centre column: simulation of Deuternaopia
      for spectrum (top centre) and cubeYF rainbow (bottom centre).
      Right column: simulation of Tritanopia for spectrum (top right) and
      cubeYF rainbow (bottom right). For the cubeYF there are no
      confusing color pairs in these simulations. There are several in the
      spectrum. Please refer to reference 14 for vcolor vision deficiency
      terminoligy. For how it was generated see my blog post:
      http://mycarta.wordpress.com/2013/02/21/perceptual-rainbow-palette-the-method/


   - Edge is based on the Goethe Edge Colors described in the book in 
     reference 3. In practice the colormap resembles a cold color map attached
     to a warm color map. But the science behind it is rigorous and the
     experimental work is based on is very intriguing to me: an alternative
     to the Newtonian spectrum. This is not perceptually balanced in a
     strict sense but because it does not have green it is perceptually
     improved in a harmonious sense (refer to paper reference 10 for a review
     of the concept of harmony in color visualization).

   Example1: 128-color rainbow with cubic-law luminance (default)
       load mandrill;
       imagesc(X);
       colormap(pmkmp(128));
       colorbar;

   Example2: 128-color palette for azimuthal data
       a=0:8:360;
       b = repmat(a,[46 1]);
       imagesc(b);
       colormap(pmkmp(128,'IsoAZ'));
       colorbar;

   See files examples.m, examples1.m, and example2.m for more examples 
   See files MakeLabPlotUsingColorspace.m and CompareLabPlotsUsingColorspace.m 
   for some demonstrations


   See also: JET, HSV, GRAY, HOT, COOL, BONE, COPPER, PINK, FLAG, PRISM,
             COLORMAP, RGBPLOT
 

   Other submissions of interest

     Matlab's new Parula colormap
     http://blogs.mathworks.com/steve/2014/10/13/a-new-colormap-for-matlab-part-1-introduction/

     Haxby color map
     www.mathworks.com/matlabcentral/fileexchange/25690-haxby-color-map
 
     Colormap and colorbar utilities
     www.mathworks.com/matlabcentral/fileexchange/24371-colormap-and-color
     bar-utilities-sep-2009
 
     Lutbar
     www.mathworks.com/matlabcentral/fileexchange/9137-lutbar-a-pedestrian-colormap-toolbarcontextmenu-creator
 
     usercolormap
     www.mathworks.com/matlabcentral/fileexchange/7144-usercolormap
 
     freezeColors
     www.mathworks.com/matlabcentral/fileexchange/7943


     Bipolar Colormap
     www.mathworks.com/matlabcentral/fileexchange/26026

     colorGray
     www.mathworks.com/matlabcentral/fileexchange/12804-colorgray

     mrgb2gray
     www.mathworks.com/matlabcentral/fileexchange/5855-mrgb2gray

     CMRmap
     www.mathworks.com/matlabcentral/fileexchange/2662-cmrmap-m

     real2rgb & colormaps
     www.mathworks.com/matlabcentral/fileexchange/23342-real2rgb-colormaps

     ColorBrewer: Attractive and Distinctive Colormaps
     http://www.mathworks.com/matlabcentral/fileexchange/45208-colorbrewer--attractive-and-distinctive-colormaps

   Acknowledgements
 
     For input to do this research I was inspired by: 
     ColorSpiral - http://bsp.pdx.edu/Software/ColorSpiral.m
     Despite an erroneous assumption about conversion/equivalence to 
     grayscale (which CMRmap achieves correctly) the main idea is ingenious
     and the code is well written. It also got me interested in perceptual
     colormaps. See reference 5 for paper
     
     For function architecture and code syntax I was inspired by:
     Light Bartlein Color Maps 
     www.mathworks.com/matlabcentral/fileexchange/17555
     (and comments posted therein)
 
     For idea on world topgraphy in examples.m I was inspired by:
     Cold color map
     www.mathworks.cn/matlabcentral/fileexchange/23865-cold-colormap

     To generate the spectrum in examples1.m I used:
     Spectral and XYZ Color Functions
     www.mathworks.com/matlabcentral/fileexchange/7021-spectral-and-xyz-color-functions
     
     For Lab=>RGB conversions I used:
     Colorspace transforamtions
     www.mathworks.com/matlabcentral/fileexchange/28790-colorspace-transformations


     For the figures in example 2 I used:
     Shaded pseudo color
     http://www.mathworks.cn/matlabcentral/fileexchange/14157-shaded-pseudo-color

     For plots in CompareLabPlotsUsingColorspace.m I used:
     cline
     http://www.mathworks.cn/matlabcentral/fileexchange/14677-cline

     For some ideas in general on working in Lab space:
     Color scale
     www.mathworks.com/matlabcentral/fileexchange/11037
     http://blogs.mathworks.com/steve/2006/05/09/a-lab-based-uniform-color-scale/

     A great way to learn more about improved colormaps and making colormaps:
     MakeColorMap
     www.mathworks.com/matlabcentral/fileexchange/17552
     blogs.mathworks.com/videos/2007/11/15/practical-example-algorithm-development-for-making-colormaps/


  References
 
     1)  Borland, D. and Taylor, R. M. II (2007) - Rainbow Color Map (Still) 
         Considered Harmful
         IEEE Computer Graphics and Applications, Volume 27, Issue 2
         Pdf paper included in submission

 
     2)  Kindlmann, G. Reinhard, E. and Creem, S. Face-based Luminance Matching
         for Perceptual Colormap Generation
         IEEE - Proceedings of the conference on Visualization '02
         www.cs.utah.edu/~gk/papers/vis02/FaceLumin.pdf
 
     3)  Koenderink, J. J. (2010) - Color for the Sciences
         MIT press, Cambridge, Massachusset
 
     4)  Light, A. and Bartlein, P.J. (2004) - The end of the rainbow? 
         Color schemes for improved data graphics.
         EOS Transactions of the American Geophysical Union 85 (40)
         Reprint of Article with Comments and Reply
         http://geography.uoregon.edu/datagraphics/EOS/Light-and-Bartlein.pdf
 
     5)  McNames, J. (2006) An effective color scale for simultaneous color
         and gray-scale publications
         IEEE Signal Processing Magazine, Volume 23, Issue1
         http://bsp.pdx.edu/Publications/2006/SPM_McNames.pdf

     6)  Rheingans, P.L. (2000), Task-based Color Scale Design
         28th AIPR Workshop: 3D Visualization for Data Exploration and Decision Making
         www.cs.umbc.edu/~rheingan/pubs/scales.pdf.gz
 
     7)  Rogowitz, B.E. and  Kalvin, A.D. (2001) - The "Which Blair project":
         a quick visual method for evaluating perceptual color maps. 
         IEEE - Proceedings of the conference on Visualization ‘01
         www.research.ibm.com/visualanalysis/papers/WhichBlair-Viz01Rogowitz_Kalvin._final.pdf
 
     8)  Rogowitz, B.E. and  Kalvin, A.D. - Why Should Engineers and Scientists
         Be Worried About Color?
         www.research.ibm.com/people/l/lloydt/color/color.HTM
 
     9)  Rogowitz, B.E. and  Kalvin, A.D. - How NOT to Lie with Visualization
         www.research.ibm.com/dx/proceedings/pravda/truevis.htm

     10) Wang, L. and Mueller,K (2008) - Harmonic Colormaps for Volume Visualization
         IEEE/ EG Symposium on Volume and Point-Based Graphics
         http://www.cs.sunysb.edu/~mueller/papers/vg08_final.pdf

     11) Wyszecki, G. and Stiles W. S. (2000) - Color Science: Concepts and 
         Methods, Quantitative Data and Formulae, 2nd Edition, John Wiley and Sons
 
     12) Niccoli, M., (2012) - How to assess a color map - in:
         52 things you should know about Geophysics, M. Hall and E. Bianco,
         eds. 

     13) Niccoli, M., and Lynch, S. (2012, in press) - A more perceptual color
         palette for structure maps, 2012 CSEG Geoconvention extended
         abstract.

     14) Color Blind Essentials eBook
         http://www.colblindor.com/color-blind-essentials/

     15) Eddins, S. (2006) - A Lab-based uniform color scale
         http://blogs.mathworks.com/steve/2006/05/09/a-lab-based-uniform-color-scale/



  Author: Matteo Niccoli
  e-mail address: matteo@mycarta.ca
  Release: 4.02
  Release date: October 2014
  Full research at:
  http://mycarta.wordpress.com/2012/05/29/the-rainbow-is-dead-long-live-the-rainbow-series-outline/
