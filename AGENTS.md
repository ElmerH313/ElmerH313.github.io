# Repository architecture

- This repository exclusively owns the `https://elmerh.com/` homepage hub.
- Keep web application source and build output out of this repository.
- A web application entry may contain only its icon, copy, and link.
- Each web application lives in an independent repository and publishes at `https://elmerh.com/<slug>/`.
- Fingerboard Web is owned by `ElmerH313/fingerboard` and linked at `/fingerboard/`; never vendor its files here.
