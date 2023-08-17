from graphviz import Digraph

# Create a new Digraph
dot = Digraph(comment='AI in Clinical Diagnosis', format='png')

# Add nodes
dot.node('Start', 'Start', shape='ellipse')
dot.node('Collect', 'Collect User Input')
dot.node('Anonymize', 'Anonymize Data')
dot.node('Diagnose', 'Use AI Model')
dot.node('Explain', 'Generate Explanation')
dot.node('Render', 'Render Template')

# Add edges
dot.edge('Start', 'Collect')
dot.edge('Collect', 'Anonymize')
dot.edge('Anonymize', 'Diagnose')
dot.edge('Diagnose', 'Explain')
dot.edge('Diagnose', 'Render')
dot.edge('Explain', 'Render')

# Save the flowchart as an image
dot.render('clinical_diagnosis_flowchart', view=True)
