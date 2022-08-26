<!-- <script> // WIP Infinite scroll with slide-in animations
    const resetScrollItems = () => {
    objectArr.set($data.work.map((obj, i) => ({ id: i, show: false })));
    document.querySelectorAll('.hscroll-item').forEach((target) => {
      observer.observe(target);
    });
  };

  const objectArr = writable([]);
  let lastIndex = $objectArr.length - 1;
  let currentIndex;
  let previousX = 0;
  let previousRatio = 0;

  const resetCarousel = () => {
    const replaceableNode = document.querySelector('.replace-me');
    const replaceableNodeClone = replaceableNode.cloneNode(true);
    console.log(replaceableNodeClone);
    replaceableNode.remove();
    $objectArr.forEach((obj, i) => {
      const node = document.getElementsByClassName(`container-${i}`)[0];
      const clone = node.cloneNode(true);
      document
        .getElementsByClassName('hscroll-container')[0]
        .appendChild(clone);
    });
    document.getElementsByClassName('hscroll-container')[0].appendChild(replaceableNodeClone);
    slideImgRight();
    currentIndex = Number(0);
    $objectArr.forEach((obj, i) => {
      document
        .getElementsByClassName('hscroll-container')[0]
        .firstChild.remove();
    });
    resetScrollItems();
  };

  const callback = (entries, observer) => {
    entries.forEach((entry) => {
      const currentX = entry.boundingClientRect.x;
      const currentRatio = entry.intersectionRatio;
      const isIntersecting = entry.isIntersecting;

      if (isIntersecting) {
        currentIndex = Number(entry.target.dataset.index);
        if (currentX > previousX && currentRatio < previousRatio) {
          console.log('Scrolling left leave');
        }
      }

      if (currentIndex === $objectArr.length - 1) {
        if (currentX < previousX) {
          if (currentRatio > previousRatio && isIntersecting) {
            console.log('Scrolling left enter');
          } else {
            // resetCarousel()
            console.log('Scrolling right leave');
          }
        } else if (currentX > previousX && isIntersecting) {
          if (currentRatio < previousRatio) {
            console.log('Scrolling left leaves');
          } else {
            console.log('Scrolling right enter');
            console.log('reload set');
          }
        }
      }

      previousX = currentX;
      previousRatio = currentRatio;
    });
  };
</script> -->