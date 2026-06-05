This page demonstrates the generation of metrics reference documentation.
{{% heading "prerequisites" %}}
{{< include "prerequisites-ref-docs.md" >}}

Clone the Kubernetes repository
The metric generation happens in the Kubernetes repository.
To clone the repository, change directories to where you want the clone to exist.
Then, execute the following command:
shell
git clone https://www.github.com/kubernetes/kubernetes
This creates a kubernetes folder in your current working directory.
Generate the metrics
Inside the cloned Kubernetes repository, locate the
test/instrumentation/documentation directory.
The metrics documentation is generated in this directory.
With each release, new metrics are added.
After you run the metrics documentation generator script, copy the
metrics documentation to the Kubernetes website and
publish the updated metrics documentation.
To generate the latest metrics, make sure you are in the root of the cloned Kubernetes directory.
Then, execute the following command:
shell
./test/instrumentation/update-documentation.sh
To check for changes, execute:
shell
git status
The output is similar to:
./test/instrumentation/documentation/documentation.md
./test/instrumentation/documentation/documentation-list.yaml
Copy the generated metrics documentation file to the Kubernetes website repository

Set the Kubernetes website root environment variable.

Execute the following command to set the website root:
shell
   export WEBSITE_ROOT=<path to website root>

Copy the generated metrics file to the Kubernetes website repository.

shell
   cp ./test/instrumentation/documentation/documentation.md "${WEBSITE_ROOT}/content/en/docs/reference/instrumentation/metrics.md"
{{< note >}}
   If you get an error, check that you have permission to copy the file.
   You can use chown to change the file ownership back to your own user.
   {{< /note >}}
Create a pull request
To create a pull request, follow the instructions in Opening a pull request.
{{% heading "whatsnext" %}}

Contribute-upstream
Generating Reference Docs for Kubernetes Components and Tools
Generating Reference Documentation for kubectl Commands