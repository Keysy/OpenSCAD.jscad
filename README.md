
<h1>Description</h1>
Some wrapper functions to ease the translation from <b><a href="http://openscad.org/">OpenSCAD</a> (.scad)</b> to <b><a href="http://joostn.github.com/OpenJsCad/">OpenJsCad</a> (.jscad)</b>: <b>OpenSCAD.jscad</b> (<tt>openscad.jscad</tt>).

<h2>History</h2>
<ul>
<li> 2013/02/28: 0.002: center:false default
<li> 2013/02/27: 0.001: first version, center: true|false support
</ul>

<h2>What Works</h2>
<ul>
<li>3d primitives: <b>sphere(), cube(), cylinder()</b>
<li>3d transformations: <b>translate(), rotate(), scale()</b>
<li>CSG operations: <b>union(), difference(), intersect()</b>
<li>what does <b>not (yet) work</b>:
<ul>
<li>cylinder(fs=<i>angle</i>)
<li>linear_extrude()
<li>rotate_extrude()
<li>2d primitives
</ul>
</ul>

<h2>Purpose</h2>
<b><a href="http://joostn.github.com/OpenJsCad/">OpenJsCad</a></b> is object oriented, and usually this imposes more verbosity of the source-code, whereas <b>OpenSCAD</b> has a simple syntax many developers are familiar with already, but unfortunately <b>OpenJsCad</b> introduced non-intuitive equivalents (essentially one has to memorize a new set of arguments), therefore a few brief wrapping functions (<tt>openscad.jscad</tt>) provide a much easier translation of existing .scad to .jscad files:

<h2>Example</h2>
<table><tr><td valign=top>
<b>.scad</b>
<pre>
union() {
      //cube(size=[30,30,0.1],center=true);
      translate([3,0,0]) cube();
      difference() {
         rotate([0,-45,0]) cube(size=[8,7,3],center=true);
         sphere(r=3,$fn=20,center=true);
      }
      translate([10,5,5]) scale([0.5,1,2]) sphere(r=5,$fn=50);
      translate([-15,0,0]) cylinder(r1=2,r2=0,h=10,$fn=20);
     
   for(i=[0:19]) {
      rotate([0,i/20*360,0]) 
      translate([i,0,0]) 
      rotate([0,i/20*90,i/20*90,0]) 
      cube(size=[1,1.2,.5],center=true);
   }
}
</pre>
</td><td valign=top width=50%>
<b>.jscad</b>
<pre>
function main() {  
   var cubes = new Array();
   for(i=0; i&lt;20; i++) {
      cubes[i] = rotate([0,i/20*360,0], 
         translate([i,0,0], 
         rotate([0,i/20*90,i/20*90,0], 
         cube({size:[1,1.2,.5],center:true}))));
   }
   return union(
      //cube({size:[30,30,0.1],center:true}),
      translate([3,0,0],cube()),
      difference(
         rotate([0,-45,0], cube({size:[8,7,3],center:true})),
         sphere({r:3,fn:20,center:true})
      ),
      translate([10,5,5], scale([0.5,1,2], sphere({r:5,fn:50}))),
      translate([-15,0,0], cylinder({r1:2,r2:0,h:10,fn:20})),
      cubes
   );
}
</pre>
</td></tr></table>

Essentially whenever named arguments in .scad appear func(a=1), translate it into func({a:1}), for example:
<ul>
<li><b>.scad:</b> <tt>translate([0,0,2]) sphere(size=2,$fn=50);</tt>
<li><b>.jscad:</b> <tt>translate([0,0,2], sphere({size:2,fn:50}));</tt>
</ul>

Also:
<pre>
   cube();                  // 1x1x1
   cube(2);                 // 2x2x2
   cube([1,2,3]);           // 1x2x3
   cube({size: [1,2,3]});   // dito
   cube({size:1, center: true});
</pre>

<h2>Example</h2>
Go to http://joostn.github.com/OpenJsCad/processfile.html and paste <tt>openscad.jscad</tt> in there.

<img src="sshot.png">

<h2>See Also</h2>
<ul>
<li><a href="https://github.com/garyhodgson/openscad.net">OpenSCAD.net</a> which provides a .scad to .js translator direct.
<li><a href="https://github.com/kaosat-dev/CoffeeSCad">CoffeeSCad</a> which provides CoffeeScript CAD development like OpenJsCad.
</ul>
            
