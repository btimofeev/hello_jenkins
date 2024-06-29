FROM btimofeev/otus-base-image:0.2.0

ENV VERSION_SDK_TOOLS "8512546_latest"
ENV VERSION_GRADLE_PROFILER "0.20.0"

ENV ANDROID_SDK_ROOT "/sdk"
ENV GRADLE_PROFILER "/gradle-profiler/gradle-profiler-${VERSION_GRADLE_PROFILER}"

ENV PATH "$PATH:$ANDROID_SDK_ROOT/tools:${GRADLE_PROFILER}/bin"

RUN echo "export ANDROID_SDK_ROOT=$ANDROID_SDK_ROOT" >> ~/.bashrc

RUN curl -s https://dl.google.com/android/repository/commandlinetools-linux-${VERSION_SDK_TOOLS}.zip > /sdk.zip && \
    mkdir -p $ANDROID_SDK_ROOT/cmdline-tools && \
    unzip /sdk.zip -d $ANDROID_SDK_ROOT && \
    rm -v /sdk.zip

ADD packages.txt $ANDROID_SDK_ROOT

RUN mkdir -p /root/.android && touch /root/.android/repositories.cfg
    
RUN ${ANDROID_SDK_ROOT}/cmdline-tools/bin/sdkmanager --update --sdk_root=$ANDROID_SDK_ROOT

RUN while read -r package; do PACKAGES="${PACKAGES}${package} "; done < /sdk/packages.txt && \
    ${ANDROID_SDK_ROOT}/cmdline-tools/bin/sdkmanager ${PACKAGES} --verbose --sdk_root=$ANDROID_SDK_ROOT

RUN ${ANDROID_SDK_ROOT}/cmdline-tools/bin/sdkmanager --list --verbose --sdk_root=$ANDROID_SDK_ROOT

RUN yes | ${ANDROID_SDK_ROOT}/cmdline-tools/bin/sdkmanager --licenses --sdk_root=$ANDROID_SDK_ROOT

RUN mkdir /gradle-profiler && \
    wget -q https://repo1.maven.org/maven2/org/gradle/profiler/gradle-profiler/${VERSION_GRADLE_PROFILER}/gradle-profiler-${VERSION_GRADLE_PROFILER}.zip -O ${GRADLE_PROFILER}.zip

RUN unzip ${GRADLE_PROFILER}.zip -d /gradle-profiler && \
    rm -v ${GRADLE_PROFILER}.zip

USER jenkins