/* =========================================================
   THE PREVENTION PROJECT
   Main JavaScript
   ========================================================= */

document.addEventListener("DOMContentLoaded", () => {

  /* =======================================================
     MOBILE NAVIGATION
     ======================================================= */

  const navToggle = document.querySelector(".nav-toggle");
  const navLinks = document.querySelector(".nav-links");

  if (navToggle && navLinks) {

    navToggle.addEventListener("click", () => {

      const isOpen =
        navToggle.getAttribute("aria-expanded") === "true";

      navToggle.setAttribute(
        "aria-expanded",
        String(!isOpen)
      );

      navLinks.classList.toggle("open");

    });


    navLinks.querySelectorAll("a").forEach(link => {

      link.addEventListener("click", () => {

        navLinks.classList.remove("open");

        navToggle.setAttribute(
          "aria-expanded",
          "false"
        );

      });

    });

  }


  /* =======================================================
     REDUCED MOTION
     ======================================================= */

  const reducedMotion =
    window.matchMedia(
      "(prefers-reduced-motion: reduce)"
    ).matches;


  /* =======================================================
     SCROLL PROGRESS
     ======================================================= */

  const progressFill =
    document.getElementById("progressFill");


  function updateProgress() {

    if (!progressFill) return;

    const scrollTop = window.scrollY;

    const documentHeight =
      document.documentElement.scrollHeight -
      window.innerHeight;

    const progress =
      documentHeight > 0
        ? scrollTop / documentHeight
        : 0;

    progressFill.style.transform =
      `scaleX(${Math.min(progress, 1)})`;

  }


  window.addEventListener(
    "scroll",
    updateProgress,
    { passive: true }
  );

  updateProgress();


  /* =======================================================
     SCROLL REVEALS
     ======================================================= */

  const revealElements =
    document.querySelectorAll(
      ".reveal, .reveal-scale, .split, .img-clip"
    );


  if (
    !reducedMotion &&
    "IntersectionObserver" in window
  ) {

    revealElements.forEach(element => {
      element.classList.add("hide");
    });


    const revealObserver =
      new IntersectionObserver(
        (entries, observer) => {

          entries.forEach(entry => {

            if (!entry.isIntersecting) return;

            entry.target.classList.remove("hide");

            observer.unobserve(
              entry.target
            );

          });

        },
        {
          threshold: 0.12,
          rootMargin:
            "0px 0px -50px 0px"
        }
      );


    revealElements.forEach(element => {
      revealObserver.observe(element);
    });

  }


  /* =======================================================
     ACTIVE NAVIGATION + DOT RAIL
     ======================================================= */

  const sections =
    document.querySelectorAll(
      "main > section[id]"
    );

  const navItems =
    document.querySelectorAll(
      ".nav-link[data-section]"
    );

  const railItems =
    document.querySelectorAll(
      ".rail-dot[data-section]"
    );


  if (
    sections.length &&
    "IntersectionObserver" in window
  ) {

    const sectionObserver =
      new IntersectionObserver(
        entries => {

          entries.forEach(entry => {

            if (!entry.isIntersecting) return;

            const id =
              entry.target.id;


            navItems.forEach(item => {

              item.classList.toggle(
                "active",
                item.dataset.section === id
              );

            });


            railItems.forEach(item => {

              item.classList.toggle(
                "active",
                item.dataset.section === id
              );

            });

          });

        },
        {
          rootMargin:
            "-35% 0px -55% 0px"
        }
      );


    sections.forEach(section => {
      sectionObserver.observe(section);
    });

  }


  /* =======================================================
     90% NUMBER COUNTER
     ======================================================= */

  const counters =
    document.querySelectorAll(
      ".count[data-target]"
    );


  if (
    counters.length &&
    "IntersectionObserver" in window
  ) {

    const counterObserver =
      new IntersectionObserver(
        (entries, observer) => {

          entries.forEach(entry => {

            if (!entry.isIntersecting) return;

            const counter =
              entry.target;

            const target =
              Number(
                counter.dataset.target
              );


            if (!Number.isFinite(target)) {
              observer.unobserve(counter);
              return;
            }


            if (reducedMotion) {

              counter.textContent = target;

              observer.unobserve(counter);

              return;

            }


            const duration = 1400;

            const startTime =
              performance.now();


            function animate(currentTime) {

              const elapsed =
                currentTime - startTime;

              const progress =
                Math.min(
                  elapsed / duration,
                  1
                );


              const eased =
                1 -
                Math.pow(
                  1 - progress,
                  3
                );


              counter.textContent =
                Math.round(
                  target * eased
                );


              if (progress < 1) {

                requestAnimationFrame(
                  animate
                );

              } else {

                counter.textContent =
                  target;

              }

            }


            requestAnimationFrame(
              animate
            );


            observer.unobserve(counter);

          });

        },
        {
          threshold: 0.5
        }
      );


    counters.forEach(counter => {
      counterObserver.observe(counter);
    });

  }


  /* =======================================================
     PARALLAX PHOTOGRAPHS
     ======================================================= */

  const parallaxImages =
    document.querySelectorAll(
      ".parallax-img"
    );


  if (
    parallaxImages.length &&
    !reducedMotion
  ) {

    let ticking = false;


    function updateParallax() {

      const viewportCenter =
        window.innerHeight / 2;


      parallaxImages.forEach(image => {

        const figure =
          image.closest(
            ".parallax-fig"
          );


        if (!figure) return;


        const rect =
          figure.getBoundingClientRect();


        /*
          Do nothing when the image is nowhere
          near the viewport.
        */

        if (
          rect.bottom < 0 ||
          rect.top > window.innerHeight
        ) {
          return;
        }


        const imageCenter =
          rect.top +
          rect.height / 2;


        const distance =
          imageCenter -
          viewportCenter;


        const movement =
          distance * -0.03;


        image.style.transform =
          `translate3d(0, ${movement}px, 0) scale(1.05)`;

      });


      ticking = false;

    }


    window.addEventListener(
      "scroll",
      () => {

        if (ticking) return;

        requestAnimationFrame(
          updateParallax
        );

        ticking = true;

      },
      { passive: true }
    );


    updateParallax();

  }


  /* =======================================================
     SMOOTH INTERNAL ANCHOR LINKS
     ======================================================= */

  document
    .querySelectorAll(
      'a[href^="#"]'
    )
    .forEach(link => {

      link.addEventListener(
        "click",
        event => {

          const targetId =
            link.getAttribute(
              "href"
            );


          if (
            !targetId ||
            targetId === "#"
          ) {
            return;
          }


          const target =
            document.querySelector(
              targetId
            );


          if (!target) return;


          event.preventDefault();


          const navbar =
            document.querySelector(
              ".navbar"
            );


          const navHeight =
            navbar
              ? navbar.offsetHeight
              : 0;


          const targetPosition =
            target.getBoundingClientRect().top +
            window.scrollY -
            navHeight;


          window.scrollTo({

            top: targetPosition,

            behavior:
              reducedMotion
                ? "auto"
                : "smooth"

          });

        }
      );

    });


  /* =======================================================
     ESCAPE KEY — CLOSE MOBILE MENU
     ======================================================= */

  document.addEventListener(
    "keydown",
    event => {

      if (event.key !== "Escape") {
        return;
      }


      if (
        !navLinks ||
        !navLinks.classList.contains("open")
      ) {
        return;
      }


      navLinks.classList.remove("open");


      navToggle?.setAttribute(
        "aria-expanded",
        "false"
      );


      navToggle?.focus();

    }
  );

});
