FROM elasticsearch:7.7.1
# Magento 2 required plugins
# https://github.com/elastic/elasticsearch-analysis-icu
# https://github.com/elastic/elasticsearch-analysis-phonetic
RUN \
    elasticsearch-plugin install analysis-icu && \
    elasticsearch-plugin install analysis-phonetic
