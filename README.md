### mrjob
---
https://github.com/Yelp/mrjob

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
























```


