FROM python:3.12-slim

ARG USERNAME=rigel
ARG USER_UID=1000
ARG USER_GID=${USER_UID}

RUN groupadd --gid ${USER_GID} ${USERNAME} \
    && useradd --uid ${USER_UID} --gid ${USER_GID} -m ${USERNAME}

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        git \
        gcc \
        clang \
        libc6-dev \
        make \
    && rm -rf /var/lib/apt/lists/*

USER ${USERNAME}
WORKDIR /workspace

ENV PATH="/home/${USERNAME}/.local/bin:${PATH}"

COPY --chown=${USERNAME}:${USERNAME} pyproject.toml ./
RUN pip install --user -e . 2>/dev/null; true

CMD ["bash"]
