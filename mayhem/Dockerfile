# Copyright 2016 Google Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
################################################################################

FROM gcr.io/oss-fuzz-base/base-builder

RUN apt-get update && apt-get install -y --no-install-recommends make autoconf automake libtool pkg-config zlib1g-dev zlib1g-dev:i386 liblzma-dev liblzma-dev:i386
COPY . libxml2
RUN rm -rf libxml2/mayhem

# The fork is missing fuzz/static_seed/regexp and fuzz/static_seed/uri;
# fetch them from upstream GNOME.
RUN git clone --depth=1 https://gitlab.gnome.org/GNOME/libxml2.git /tmp/libxml2-upstream \
    && cp -r /tmp/libxml2-upstream/fuzz/static_seed /src/libxml2/fuzz/ \
    && rm -rf /tmp/libxml2-upstream

WORKDIR libxml2
COPY mayhem/build.sh $SRC/

ENV FUZZING_LANGUAGE=c++ SANITIZER=address FUZZING_ENGINE=libfuzzer ARCHITECTURE=x86_64
RUN compile
RUN chmod +x /out/html
