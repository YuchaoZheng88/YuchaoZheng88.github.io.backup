# Module 19: Cloud Computing

- infrastructure-as-a-service (IaaS)
- platform-as-a-service (PaaS)
- software-as-a-service (SaaS)

## Perform S3 Bucket Enumeration

### Enumerate S3 Buckets using lazys3
  
  lazys3 is a Ruby script tool that is used to brute-force AWS S3 buckets using different permutations. 
  
  1. ``` cd lazys3 ```
  2. ``` ruby lazys3.rb ```
  3. ``` ruby lazys3.rb [Company]  ```
    You can search the S3 buckets of specific company.

### Enumerate S3 Buckets using S3Scanner

  1. ``` cd S3Scanner ```
  2. ``` pip3 install -r requirements.txt ```
  3. ``` python3 ./s3scanner.py sites.txt ``` sites.txt is a text file containing the target website URL that is scanned for open S3 buckets.
  4. ``` python3 ./s3scanner.py --include-closed --out-file found.txt --dump names.txt ``` Dump all open buckets and log both open and closed buckets in found.txt.
  5. ``` python3 ./s3scanner.py names.txt ``` log open buckets in the default output file (buckets.txt)
  6. ``` python ./s3scanner.py --list names.txt ``` Save the file listings of all open buckets to a file

### Other tools:
  - S3Inspector (https://github.com)
  - s3-buckets-bruteforcer (https://github.com)
  - Mass3 (https://github.com)
  - Bucket Finder (https://digi.ninja)
  - s3recon (https://github.com)
    
## Exploit S3 Buckets

  
