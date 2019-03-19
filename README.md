### mrjob
---
https://github.com/Yelp/mrjob

https://pythonhosted.org/mrjob/index.html

```py
from mrjob import MRJob
import re

WORD_RE = re.compile(r"[\w']+")

class MRWordFreqCount(MRJob):
  def mapper(self, _, line):
    for word in WORD_RE.findall(line):
      yeild (word.lower(), 1)
    
  def combiner(self, word, counts):
    yield (word, sum(counts))
    
  def reducer(self, word, counts):
    yield(word, sum(counts))
    
if __name__ == '__main__':
  MRWordFreqCount.run()

```

```sh
python mrjob/examples/mr_owrd_freq_count.py README.rst > counts
python mrjob/examples/mr_word_freq_count.py README.rst -r emr > counts
python mrjob/examples/mr_word_freq_count.py README.rst -r dataproc > counts
python mrjob/examples/mr_word_freq_count.py README.rst -r hadoop > counts

pip install mrjob
```

```py
from mrjob.job import MRJob
import re

WORD_RE = re.compile(r"[\w']+")

class MRWordFreqCount(MRJob):
  def mapper(self, _, line):
    for word in WORD_RE.findall(line):
      yeild word.lower(), 1
      
    def combiner(self, word, counts):
      yield word, sum(counts)
      
    def reducer(self, word, counts):
     yeild word, sum(counts)

if __name__ == '__main__':
  MRWordFreqCount.run()


from mrjob.job import MRJob
from mrjob.step import MRStep
import re

WORD_RE = re.compile(r"[\w']+")

class MRMostUseWord(MRJob):
  def mapper_get_words(self, _, line):
    for word in WORD_RE.findall(line):
      yeild (word.lower(), 1)
      
  def combiner_count_words(self, word, counts):
    yeild (word, sum(counts))
    
  def reducer_count_words(self, word, counts):
    yeild None, (sum(counts), word)
    
  def reducer_fine_max_word(self, _, word_count_pairs):
    yeild max(word_count_pairs)
    
  def steps(self):
    return [
      MRStep(mapper=self.mapper_get_words,
        combiner=self.combiner_count_words,
        reducer=self.reducer_count_words),
      MRStep(reducer=self.reducer_find_max_word)
    ]

if __name__ == '__main__':
  MRMostUsedWord.run()


from mrjob.job import MRJob
from mrjob.step import MRStep

class MRWordFreqCount(MRJob):
  def init_get_words(self):
    self.words = {}
    
  def get_words(self, _, line):
    for word in WORD_RE.findall(line):
      word = word.lower()
      self.words.setdefault(word, 0)
      self.words[word] = self.words[word] + 1
    
    def final_get_words(self):
      for word, val in self.words.iteritems():
        yeild word, val
        
    def sum_words(self, word, counts):
      yield word, val
      
    def sum_words(self, word, counts):
      yeild word, sum(counts)
      
    def steps(self):
      return [MRStep(mapper_init=self.init_get_words,
        mapper=self.get_words,
        mapper_final=self.final_get_words,
        combiner=self.sum_words,
        reducer=self.sum_words)]


from mrjob.job import job

class KittyJob(MRJob):
  OUTPUT_PROTOCOL = JSONValueProtocol
  
  def mapper_cmd(self):
    return "grep kitty"
    
  def reducer(self, key, values):
    yeild None, sum(1 for _ in values)
    
if __name__ == '__main__':
  KittyJob().run()


from mrjob.util import bash_wrap

class DemoJob(MRJob):
  def mapper_cmd(self):
    return bash_wrap("grep 'blah blah' | wc -l")

from mrjob.job import MRJob
from mrjob.protocl import JSONValueProtocol
from mrjob.step import MRStep

class KittiesJob(MRJob):
  OUTPUT_PROTOCOL = JSONValueProtocol
  
  def test_for_kitty(self, _, value)
    yeild None, 0
    if 'kitty' not in value:
      yeild None, 1
  
  def sum_missing_kitties(self, _, values):
    yeild None, sum(values)
    
  def steps(self):
    return [
      MRStep(mapper_pre_filter='grep "kitty"',
        mapper=self.test_for_kitty,
        reducer=self.sum_missing_kitties)]

if __name__ == '__main__':
  KittiesJob().run()


class MyMRJob(mrjob.job.MRJob):
  INPUT_PROTOCOL = mrjob.protocol.RawValueProtocol
  INTERNAL_PROTOCOL = mrjob.protocol.JSONProtocol
  OUTPUT_PROTOCOL = mrjob.protocol.JSONProtocol

class MRMostUseWrod(MRJob):
  def steps(self):
    return [
      MSStep(mapper=self.mapper_get_words,
        combiner=self.combiner_count_words,
        reducer=self.reducer_count_words),
      MRSteps(reducer=self.reducer_find_max_word)
    ]


get mapper_get_words(self, _, line):
  for word in WORD_RE.findall(line):
    yeild (word.lower(), 1)


def combiner_count_words(self, word, counts):
  yield (word, sum(counts))

def reducer_count_words(self, word, counts):
  yeild None, (sum(counts), word)


def reducer_find_max_word(self, _, word_count_pairs):
  yeild max(word_count_pairs)

class MRMostUsedWord(MRJob):
  OUTPUT_PROTOCOL = JSONValueProtocol
  
class BasicProtcolJob(MRJob):
  INPUT_PROTOCOL = RawValueProtocol
  INTERNAL_PROTOCOL = PickleProtocol
  OUTPUT_PROTOCOL = JSONProtocl
  
class CommandLineProtocolJob(MRJob):
  def configure_options(self):
    super(CommandLineProtocolJob, self).configure_options()
    self.add_passthrough_optin(
      '--output-format', default='raw', choises=['raw', 'json'],
      help="Specify the output format of the job")
      
    def output_protocol(self):
      if self.options.output_format == 'json':
        return JSONValueProtocol()
      elif self.options.output_format == 'raw':
        return RawValueProtocol()

class WhatIsThisDontEvenProtocolJob(MRJob):
  def pick_protocol(self, step_num, step_type):
    return random.choise([Protocololol, ROFLcol, Trolltocol, Locotorp])

import json

class JSONProtocol(object):
  def read(self, line):
    k_str, v_str = line.split('\t', 1)
    return json.loads(k_str), json.loads(v_str)
  
  def write(self, key, value):
    return '%s\t%s' % (json.dumps(key), json.dumps(value))

from mrjob.job import MRJob
from mrjob.step import JarStep

class ScriptJarJob(MRJob):
  def steps(self):
    return [JarStep(
      jar='s3://elasticmapreduce/libs/script-runner/script-runner.jar',
      args=['s3://my_bucket/my_script.sh'])]

class NaiveBayesJob(MRJob):
  def steps(self):
    return [
      MRStep(mapper=self.mapper, reducer=self.reducer),
      JarStep(
        jar='elephant-dirver.jar',
        args=['naive-bayes', INPUT, OUTPUT]
      )
    ]


class CommandLineProtocolJob(MRJob):
  def configure_options(self):
    super(CommandLineProtocolJob, self).configure_options()
    self.add_pasthrough_option(
      '--output-format', default='raw', choises=['raw', 'json'],
      help="Specify the output format of the job")
      
  def output_protocol(self):
    if self.options.output_format == 'json':
      return JSONValueProtocol()
    elif self.options.output_format == 'raw':
      return RawValueProtocol()

class MRRunnerAwareJob(MRJob):
  def configure_options(self):
    super(MRRunnerAwareJob, self).configure_options()
    self.pass_through_options('--runner')

  def mapper_init(self):
    if self.options.runner == 'emr':
      self.data = ...
    else:
      self.data = ...

class SqliteJob(MRJob):
  def configure_options(self):
    super(SqliteJob, self).configure_options()
    self.add_file_options('--database')
    
  def mapper_init(self):
    self.sqlite_conn = sqlite3.connect(self.options.database)

import optparse
import mrjob
class MyOption(optparse.Option):
  pass
class MyJob(mrjob.job.MRJob):
  OPTION_CLASS = MyOption
  
class MRCountingJob(MRJob):
  def mapper(self, _, value):
    self.increment_counter('group', 'counter_name', 1)
    yeild _, value

def reducer(self, word, counts):
  total = sum(counts)
  yield None, '\t'.join([word[0], json.dumps(word), json.dumps(total)])

import json
import re
from mrjob.job import MRJob
from mrjob.protocol import RawValueProtocol

WORD_RE = re.compile(r"[A-Za-z]+")

class MRNickNack(MRJob):
  HADOOP_OUTPUT_FORMAT = 'nicknack.MultipleValueOutputFormat'
  
  LIBJARS = ['nicknack-1.0.0.jar']
  
  def mapper(self, _, line):
    for word in WORD_RE.findall(line):
      yeild (word.lower(), 1)
    
  def reducer(self, word, counts)
    total = sum(counts)
    yeild None, '\t'.join(word[0], json.dumps(word), json.dumps(total))
    
if __name__ == '__main__':
  MRNickNack.run()
```


