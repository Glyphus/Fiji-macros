setBatchMode(true);
starttime = getTime();
run("Bio-Formats Macro Extensions");
dir = getDirectory("Choose a Directory ");
run("Options...", "iterations=1 count=1 black");
run("Colors...", "foreground=white background=black selection=yellow"); //set colors
run("Set Measurements...", "area mean median display redirect=None decimal=5");
if (roiManager("count")>0) {
					roiManager("Delete"); }
run("Clear Results");				
files = newArray(0);
files = find(dir, files);
files = getFilteredList(files);

  function find(dir, files) {
      list = getFileList(dir);
      for (i=0; i<list.length; i++) {
      	if (endsWith(list[i], "/"))
            files = find(""+dir+list[i], files);
		else if (endsWith(list[i], ".tif"))
			files = Array.concat(files,""+dir+list[i]); }
		return files;
  }
  
  function getFilteredList(files) {
  	returnedList = newArray(0);
  	Filter = newArray("MAX");
	for (i = 0; i < files.length; i++) {
		for (j = 0; j < Filter.length; j++){
			if (indexOf(files[i],Filter[j]) != -1) returnedList = Array.concat(returnedList,files[i]);
	}
	}
	return returnedList;
  }
for (f=0; f<files.length; f++) {
	Ext.openImagePlus(files[f]);
	name=getTitle;
	run("Duplicate...", "title=[nucleidetection]");
	setAutoThreshold("Huang dark");
	run("Convert to Mask");
	run("Analyze Particles...", "size=1000-Infinity show=Nothing add include");
	n=roiManager("Count");
	for (k=0; k<roiManager("Count"); k++) {
		selectWindow(name);
		roiManager("Select",k);
		run("Enlarge...", 10);
		run("Duplicate...", "duplicate");
		rename(k);
		selectWindow(name);
		run("Clear", "stack");
	}
	roiManager("Deselect");
	roiManager("Delete");
	for (i=0; i<n; i++) {
		selectWindow(i);
		run("Z Project...", "projection=[Max Intensity]");
		setAutoThreshold("MaxEntropy dark");
		run("Convert to Mask");
		run("Analyze Particles...", "size=20-1000 show=Nothing add exclude");
		if (roiManager("Count")>0) {
			run("Select All");
			run("Clear");
			for (j=0; j<roiManager("Count"); j++){
				roiManager("select", j);
				run("Fill");
			}
			roiManager("Deselect");
			roiManager("Delete");
			run("Select None");
			run("Convert to Mask");
			run("Create Selection");
			roiManager("Add");
			run("Select None");
			run("Dilate");
			run("Dilate");
			roiManager("select", 0);
			run("Clear");
			run("Convert to Mask");
			run("Select None");
			run("Create Selection");
			roiManager("Add");
			roiManager("Deselect");
			selectWindow(i);
			run("Select None");
			roiManager("multi-measure measure_ALL");
			saveAs("Measurements", dir+name+" cell "+i+".txt");
			roiManager("Delete");
			run("Clear Results");
		}
	}
	run("Close All");
	l=l+i;
}
print(f+" files and "+l+" nuclei in "+(getTime()-starttime)/1000+" seconds analyzed!");
