### Convert images

OSX:

    $ cd assets; for i in *.jpg; do sips -Z 350 $i --out min/$i;done

Ubuntu (graphicsmagick):

    $ cd assets
    $ gm convert name.png -resize 350 min/name.png

### Generate puml diagrams

    $ plantuml assets/network.puml
