Steps for image processing

Source files are located on Dropbox at Ben/CHE50/Tech/Art/computing FINAL/source_files and Ben/CHE50/Tech/Art/computing FINAL/source_files_keepquality
create all the directories since mogrify won't do this mkdir opt inter huge large tablet mobile
reduce LARGE jpeg quality to 60 mogrify -path opt  -strip -quality 60 source_files/*.jpg
copy files in source_files_keepquality\ to opt, too cp source_files_keepquality/*.jpg opt/
convert JPEGS to use progressive loading mogrify -path inter -interlace Plane opt/*.jpg
make the different image sizes we want for mobile, tablet, desktop and large displays. the resize will only apply to images LARGER THAN the specified dimensions because of the \> command
mogrify -path huge -resize 2000x2000\> *.png
mogrify -path large -resize 1500x1500\> *.png
mogrify -path tablet -resize 959x959\> *.png
mogrify -path mobile -resize 480x480\> *.png
upload the resulting directories (huge, large, tablet, mobile) to S3 so they can be served to the page
Other useful commands:
print out details about images identify *.jpg
check if image uses progressive loading identify -verbose file.jpg | grep Interlace. if you get "Interlace: JPEG", it is progressive, if you get "Interlace: None" it isn't


mogrify -path transparent -bordercolor white -border 1x1 \
          -alpha set -channel RGBA -fuzz 10% \
          -fill none -floodfill +0+0 white \
          -shave 1x1 1890s_*.png