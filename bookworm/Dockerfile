###########################################################
# Dockerfile that builds a Silica Gameserver
###########################################################

# BUILD STAGE

FROM cm2network/steamcmd:root-bookworm as build_stage

LABEL maintainer="joedwards32@gmail.com"

ENV STEAMAPPID 2738040
ENV STEAMAPP silica
ENV STEAMAPPDIR "${HOMEDIR}/${STEAMAPP}-dedicated"
ENV STEAMAPPVALIDATE 0

COPY etc/entry.sh "${HOMEDIR}/entry.sh"
COPY etc/pre.sh "/etc/pre.sh"
COPY etc/post.sh "/etc/post.sh"

RUN set -x \
	# Install, update & upgrade packages
	&& apt-get update \
	&& apt-get install -y --no-install-recommends --no-install-suggests \
		wget \
		ca-certificates \
		lib32z1 \
                libicu-dev \
                gettext \
	&& mkdir -p "${STEAMAPPDIR}" \
	# Add entry script
	&& chmod +x "${HOMEDIR}/entry.sh" \
	&& chown -R "${USER}:${USER}" "${HOMEDIR}/entry.sh" "${STEAMAPPDIR}" \
	# Clean up
        && apt-get clean \
        && find /var/lib/apt/lists/ -type f -delete
	
# BASE

FROM build_stage AS bookworm-base

COPY etc/server.template.xml "/etc/server.template.xml"

ENV SERVER_NAME="test server" \
    CURRENT_GAME_MODE="MP_Strategy" \
    SINGLEPLAYER="false" \
    PASSWORD_PROTECTED="false" \
    SERVER_PASSWORD="true" \
    DEATHMATCH_MAP="MonumentValley" \
    DEATHMATCH_MAP_LIST="Badlands CombatDome GreatErg MonumentValley NarakaCity ProvingGrounds RiftBasin RiftBasinTerrainOnly" \
    DEATHMATCH_MAX_PLAYERS=32 \
    PROSPECTOR_MAP="RiftBasin" \
    PROSPECTOR_MAP_LIST="ProspectorTest RiftBasin" \
    PROSPECTOR_MAX_PLAYERS=4 \
    SANDBOX_MAP="MonumentValley" \
    SANDBOX_MAP_LIST="AssetTest Badlands CombatDome GreatErg MainMenuNoMenu MonumentValley NarakaCity ProspectorTest ProvingGrounds RiftBasin SiegeMap SmallStrategyTest" \
    SANDBOX_MAX_PLAYERS=12 \
    STRATEGY_MAP="MonumentValley" \
    STRATEGY_MAP_LIST="Badlands GreatErg MonumentValley NarakaCity RiftBasin RiftBasinTerrainOnly SmallStrategyTest" \
    STRATEGY_MAX_PLAYERS=12 \
    TOWER_DEFENSE_MAP="SiegeMap" \
    TOWER_DEFENSE_MAP_LIST="SiegeMap SmallStrategyTest" \
    TOWER_DEFENSE_MAX_PLAYERS=12

# Set permissions on STEAMAPPDIR
#   Permissions may need to be reset if persistent volume mounted
RUN set -x \
        && chown -R "${USER}:${USER}" "${STEAMAPPDIR}" \
        && chmod 0777 "${STEAMAPPDIR}"

# Switch to user
USER ${USER}

WORKDIR ${HOMEDIR}

CMD ["bash", "entry.sh"]

# Expose ports
EXPOSE 27015/tcp \
	27015/udp \
	27016/tcp \
	27016/udp
