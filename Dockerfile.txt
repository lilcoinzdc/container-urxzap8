# Use the latest Ubuntu image
FROM ubuntu:latest

# Update and install required packages
RUN apt-get update && \
    apt-get install -y git software-properties-common curl && \
    add-apt-repository ppa:deadsnakes/ppa -y && \
    apt-get update && \
    apt-get install -y python3.10 python3.10-distutils && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install pip for Python 3.10
RUN curl -sS https://bootstrap.pypa.io/get-pip.py -o get-pip.py && \
    python3.10 get-pip.py && \
    rm get-pip.py

# Set the working directory
WORKDIR /app

# Clone the repository
RUN git clone https://github.com/oobabooga/text-generation-webui.git

# Set the working directory to the cloned repository
WORKDIR /app/text-generation-webui

# Install Python dependencies
RUN python3.10 -m pip install -r requirements.txt

# Make the start_linux.sh script executable
RUN chmod +x start_linux.sh 
# Define the command to be run when the container starts
CMD ["bash", "/app/text-generation-webui/start_linux.sh --cpu --share"]
