# ----------------------------------------
# SENSOR DATA PARSING - FULL SETUP SCRIPT
# ----------------------------------------

# 1. Connect to the server (from your local Git Bash terminal)
ssh -L 8061:localhost:8061 your_username@192.168.101.100

# 2. (FIRST TIME ONLY) Install MicroMamba for environment management
"${SHELL}" <(curl -L micro.mamba.pm/install.sh)

# After installation, exit and reconnect
exit

# 3. Reconnect to the server again
ssh -L 8061:localhost:8061 your_username@192.168.101.100

# 4. Verify micromamba installation
micromamba

# 5. (FIRST TIME ONLY) Create a virtual environment named `sensor_data_parse_venv` with Python 3.10
micromamba create -n sensor_data_parse_venv python=3.10

# 6. Activate the environment
micromamba activate sensor_data_parse_venv

# 7. (FIRST TIME ONLY) Install Jupyter kernel support
micromamba install ipykernel || pip install ipykernel

# 8. Register this environment as a Jupyter kernel
python -m ipykernel install --user --name sensor_data_parse_venv --display-name "sensor_data_parse_venv"

# 9. (FIRST TIME ONLY) Generate a GitHub Personal Access Token
#    -----------------------------------------------------------
#    Follow these steps on GitHub (done in your browser):
#    1. Log in to GitHub.
#    2. Go to Settings → Developer Settings → Personal Access Tokens → Tokens (classic).
#    3. Click "Generate new token" → "Generate new token (classic)".
#    4. Give it a name, set an expiration (e.g. 90 days).
#    5. Select the "repo" scope.
#    6. Click "Generate token" and copy it immediately.
#    7. Use this token instead of your GitHub password when cloning or pushing.
#    ⚠️ Treat it like a password. Do not share or commit it.
#    -----------------------------------------------------------

# 10. Make your project directory and move into it
mkdir -p ~/scai_data_process/data_parse
cd ~/scai_data_process/data_parse

# 11. Clone the Sensor-Data-Parsing repo (you'll be prompted for your GitHub username and the token as password)
git clone https://github.com/SCAI-Lab/Sensor-Data-Parsing.git

# 12. Move into the cloned repo
cd Sensor-Data-Parsing

# 13. (RECOMMENDED) Create your own working branch
#     Replace "your-branch-name" with something meaningful (e.g., your name or task)
git checkout -b your-branch-name

# 14. Install project dependencies in editable mode
pip install -e .

# 15. (Optional) Set your Git identity if you plan to commit/push
git config --global user.email "your.email@example.com"
git config --global user.name "Your Name"

# 16. (Optional) When ready to save your changes
# git add .
# git commit -m "Your commit message"
# git push origin your-branch-name

# 17. Run the parser script
python -m parselib.raw2interim

# 18. (Optional) Start Jupyter Notebook
jupyter notebook --ip=127.0.0.1 --port=8061 --no-browser
# → Copy one of the links shown and paste it into your browser.
# → Switch the kernel to "sensor_data_parse_venv" before running notebooks.
