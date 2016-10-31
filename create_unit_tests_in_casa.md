# Pipeline unit tests

Follow these steps to create and test a new pipeline unit test

1. Get and unpack latest tarball

    ```
    cp /home/casa/distro/linux/test/el6/casa-test-5.0.51.tar.gz .
    tar xvzf casa-test-5.0.51.tar.gz
    ```

1. Set CASAPATH

    ```
    CASAPATH=`pwd`/casa-test-5.0.51
    ```

1. Add a new test

    *For my workflow*
    ```
    touch ~/lustre/unittests/tests/test_hello_world.py
    cd $CASAPATH/lib/python2.7/tests/
    ln -s ~/lustre/unittests/tests/test_hello_world.py .
    ```
    
    For others
    ```
    touch $CASAPATH/lib/python2.7/tests/test_hello_world.py
    ```
    
    test_<module_name>.py should be structured like this.
    ```python
    import unittest
    
    class MyTestCase(unittest.TestCase):
        def test_default_greeting_set(self):
            self.assertEqual('Hello world!', 'Hello world!')
    
    def suite():
        return [MyTestCase]
    ```

1. Add test data (optional)

    Add data files to this directory.
    ```
    mkdir -p ~/lustre/unittests/data/hello_world
    ```

1. Add test to list of tests (optional, but useful for running all tests)

    Edit ```~/lustre/unittests/tests/unittests_list.txt``` and append
    ```
    test_hello_world		jmasters@nrao.edu
    ```
    
1. Run

    Individual test file
    ```
    $CASAPATH/bin/casa --nogui --nologger -c $CASAPATH/lib/python2.7/regressions/admin/runUnitTest.py test_hello_world
    ```
    
    If we need to use a data input directory
    ```
    $CASAPATH/bin/casa --nogui --nologger -c $CASAPATH/lib/python2.7/regressions/admin/runUnitTest.py \
    --datadir ~/lustre/unittests/data/ test_hello_world
    ```
    
    All tests in ```unittests_list.txt```
    ```
    $CASAPATH/bin/casa --nogui --nologger -c $CASAPATH/lib/python2.7/regressions/admin/runUnitTest.py \
    --datadir ~/lustre/unittests/data/ --file ~/lustre/unittests/tests/unittests_list.txt
    ```

## Note
Running the test creates the ```nosedir``` directory, which  contains test results.

See http://www.eso.org/~scastro/ALMA/CASAUnitTests.htm for more information and further integration into CASA.