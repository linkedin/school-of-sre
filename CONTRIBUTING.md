## Contributing Guidelines

Ensure that you adhere to the following guidelines:

- Should be about principles and concepts that can be applied in any company or individual project. Do not focus on particular tools or tech stack(which usually change over time).
- Adhere to the Code of Conduct.
- Should be relevant to the roles and responsibilities of an SRE.
- Should be locally tested (see steps for testing) and well formatted.
- It is good practice to open an issue first and discuss your changes before submitting a pull request. This way, you can incorporate ideas from others before you even start.

### Building and testing locally

Run the following commands to build and view the site locally before opening a PR.     
  
    python3 -m venv .venv  
    source .venv/bin/activate  
    pip install -r requirements.txt  
    mkdocs build  
    mkdocs serve  
  
  

Opening a PR

Follow [GitHub PR workflow](https://guides.github.com/introduction/flow/) for your contribution.

Fork this repo, create a feature branch, commit your changes and open a PR to this repo.
