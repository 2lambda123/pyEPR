Examples
=========

Start-up quick example
-----------------------

The following code illustrates how to perform a complete analysis of a
simple two-qubit, one-cavity device in just a few lines of code with
``pyEPR``. In the HFSS file, before running the script, first specify
the non-linear junction rectangles and variables (see Sec. pyEPR Project
Setup in HFSS). All operations in the eigen analysis and Hamiltonian
computation are fully automated. The results are saved, printed, and
succinctly plotted.

.. code:: python

   # Load pyEPR. See the tutorial notebooks!
   import pyEPR as epr

   # 1. Connect to your Ansys, and load your design
   pinfo = epr.ProjectInfo(project_path = r'C:\sim_folder',
                            project_name = r'cavity_with_two_qubits',
                            design_name  = r'Alice_Bob')

   # 2. Non-linear (Josephson) junctions: specify
   pinfo.junctions['jAlice'] = {'Lj_variable':'LJAlice', 'rect':'qubitAlice', 'line': 'alice_line', 'length':parse_units('50um')}
   pinfo.junctions['jBob']   = {'Lj_variable':'LJBob',   'rect':'qubitBob',   'line': 'bob_line',   'length':parse_units('50um')}
   pinfo.validate_junction_info() # Check that valid names of variables and objects have been supplied.

   # 2b. Dissipative elements: specify
   pinfo.dissipative.dielectrics_bulk    = ['si_substrate', 'dielectic_object2'] # supply names of hfss objects
   pinfo.dissipative.dielectric_surfaces = ['interface1', 'interface2']

   # 3.  Perform microwave analysis on eigenmode solutions
   eprh = epr.DistributedAnalysis(pinfo)
   eprh.do_EPR_analysis()

   # 4.  Perform Hamiltonian spectrum post-analysis, building on mw solutions using EPR
   epra = epr.QuantumAnalysis(eprh.data_filename)
   epra.analyze_all_variations(cos_trunc = 8, fock_trunc = 7)
   epra.plot_hamiltonian_results()

Video tutorials
------------------------------------------

.. raw:: html

    <div style="overflow:auto;">
    <table style="">
    <tr>
        <th>
        <a href="https://www.youtube.com/watch?v=fSRYvD-ITnQ&list=PLnak_fVcHp17tydgFosNtetDNjKbEaXtv&index=1">
        Tutorial 1 - Overview
        <br>
        <img src="https://img.youtube.com/vi/fSRYvD-ITnQ/0.jpg" alt="pyEPR Tutorial 1 - Overview" width=250>
        </a>
        </th>
        <th>
        <a href="https://www.youtube.com/watch?v=ZTi1pb6wSbE&list=PLnak_fVcHp17tydgFosNtetDNjKbEaXtv&index=2">
        Tutorial 2 - Setup of Conda & Git
        <br>
        <img src="https://img.youtube.com/vi/ZTi1pb6wSbE/0.jpg" alt="pyEPR Tutorial 2 - Setup of Conda & Git" width=250>
        </a>
        </th>
        <th>
        <a href="https://www.youtube.com/watch?v=L79nlXY2w4s&list=PLnak_fVcHp17tydgFosNtetDNjKbEaXtv&index=3">
        Tutorial 3 - Setup of Packages & Config
        <br>
        <img src="https://img.youtube.com/vi/L79nlXY2w4s/0.jpg" alt="pyEPR Tutorial 3 - Setup of Packages & Config" width=250>
        </a>
        </th>
    </tr>
    </table>
    </div>



Jupyter notebooks and example Ansys files
------------------------------------------

The most extensive way to learn pyEPR is to look over the pyEPR `example notebooks`_ and `example files`_ contained in the package repo.

The Jupyter notebooks can be viewed in a web browser `directly here`_.

.. _example notebooks: https://github.com/zlatko-minev/pyEPR/tree/master/_tutorial_notebooks
.. _example files: https://github.com/zlatko-minev/pyEPR/tree/master/_example_files
.. _directly here: https://nbviewer.jupyter.org/github/zlatko-minev/pyEPR/tree/master/_tutorial_notebooks/