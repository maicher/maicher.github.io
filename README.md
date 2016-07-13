### Convert images

    cd assets; for i in *.jpg; do sips -Z 350 $i --out min/$i;done