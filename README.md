# ofs

1. Go to the media page, and select any of the All, Photo or Video, depending on what you want
2. Press F12 to open the console (Chrome)
3. Copy the following code into the console and press enter:
    ```
    window.checkedObjects = []

    function search(path, child, obj){
        try {
            if(obj === null || obj === undefined || child === 'document'){
                return false;
            }
            for(let i=0; i< window.checkedObjects.length;i++){
                if(obj === window.checkedObjects[i]){
                    return false;
                }
            }
            window.checkedObjects.push(obj);
            if(obj.hasOwnProperty('$el')){
                if(obj['$el']._prevClass === 'b-photos g-negative-sides-gaps'){
                    console.log("found container");
                    window.myobj = obj;
                    return true;
                }
            }

            for(child in obj){
                let childPath = '';
                if(!isNaN(child)){
                    childPath = path + '[' + child + ']';
                }
                else{
                    childPath = path + "." + child;
                }
                if(search(childPath, child, obj[child])){
                    return true;
                }
            }
        } catch (error) {

        }
    }

    function scroll() {
        setTimeout(() => {
            window.currentScrollCount++;
            console.log("scroll " + window.currentScrollCount.toString() + "/" + window.scrollCount.toString());
            window.scrollTo(0, document.body.scrollHeight);
            if(window.currentScrollCount < window.scrollCount) {
                scroll();
            }
            else {
                console.log("done scrolling");
            }
        }, 2000);
    }

    function startScroll(scrollCount) {
        window.scrollCount = scrollCount;
        window.currentScrollCount = 0;
        scroll();
    }

    function getCount() {
        console.log("collected " + myobj.photoSwipeItems.length + " images/videos");
    }

    function getLinks(startIndex, endIndex) {
        let allLinks = "";
        if(startIndex === undefined || startIndex < 0) {
            startIndex = 0;
        }
        if(endIndex === undefined || endIndex >= myobj.photoSwipeItems.length) {
            endIndex = myobj.photoSwipeItems.length -1;
        }
        for(let i = startIndex; i <= endIndex; i++) {
            if(myobj.photoSwipeItems[i].src === undefined) {
                allLinks += myobj.photoSwipeItems[i].media.source.source + "\n";
            }
            else {
                allLinks += myobj.photoSwipeItems[i].src + "\n";
            }
        }
        setTimeout(() => {
            navigator.clipboard.writeText(allLinks);
            console.log("copied links to clipboard");
        }, 3000);
    }
    ```
4. Find the container object which will contain all the links

    - Copy this code into console, press enter and wait until a message "*found container*" appears (this may take half a minute or so)
  
      ```search("window", "", window)```

5. At this moment you only have the first 30 or so links, if you want more you need to scroll down as far as you want in order for more links to load
 
    - you can do this automatically by copying this command into console and pressing enter, change the number depending on how many times you want to scroll
  
      ```startScroll(2)```
    
6. You can check the amount of links you have at this moment by copying this command to console and pressing enter:
      
      ```getCount()```
    
7. To copy these links to your clipboard enter this command (IMPORTANT: after pressing enter, click with your mouse back somewhere on the website to focus on it within 3 seconds of pressing enter)
    
      ```getLinks()```
      
8. Open any text editor (for example notepad), and just paste the links into it, and save the file as, for example, links.txt to your Downloads folder

9. Additionaly you can specify the start and end index of the links which you want

      ```getLinks(3, 9)``` - this would copy only the links between the 3rd and 9th link
      
10. Now that you have your links saved on your Downloads folder as links.txt it is time to download them

    - Go to the Downloads folder, press and hold the shift key, and right click with your mouse on an empty place in the folder
    - Press Open PowerShell window here
