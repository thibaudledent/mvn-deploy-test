# mvn-deploy-test
Test the `deployAtEnd=true` during `mvn release:perform` and `mvn deploy`.

## Testing `mvn prepare` and `mvn deploy`

I did some tests with `deployAtEnd=true`, the download and the upload of files are done at the end (check the diff the output with `deployAtEnd` in the main `pom.xml` and without it, search for "_at end_").

### How to use deployAtEnd?

#### Solution 1
Add the configuration in the `pom.xml`:
```
<plugin>
    <artifactId>maven-deploy-plugin</artifactId>
    <version>2.8.2</version>
    <configuration>
        <deployAtEnd>true</deployAtEnd>
    </configuration>
</plugin>
```

(example [here|https://github.com/thibaudledent/mvn-deploy-test/commit/eceec95fbf97b168eb7c2ca41eacb72026d62a82#diff-600376dffeb79835ede4a0b285078036R46])

#### Solution 2

```mvn -Darguments=\"-DdeployAtEnd=true\" release:perform```

It does the same, see the following files and search for "_[INFO] [INFO] Deploying org.example:mvn-deploy-test:X.X *at end*_":


### Step to reproduce

1) Clone the repo https://github.com/thibaudledent/mvn-deploy-test
2) Add the config or not in the main `pom.xml`:
```
<plugin>
    <artifactId>maven-deploy-plugin</artifactId>
    <version>2.8.2</version>
    <configuration>
        <deployAtEnd>true</deployAtEnd>
    </configuration>
</plugin>
```
3) `mvn release:prepare`
4) `mvn release:perform`
5) Remove the config in the main `pom.xml`
6) `mvn release:prepare`
7) `mvn -Darguments=\"-DdeployAtEnd=true\" release:perform`
8) Check the output, it does the same
